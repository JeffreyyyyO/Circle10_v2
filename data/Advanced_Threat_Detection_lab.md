# **CYS 535—WEEK 3—LAB**

## **ADVANCED THREAT DETECTION**

1. # **Configuring Advanced Detection Rules**

## **Objective**

In this lab, you will learn how to design, configure, and implement advanced security detection rules using correlation capabilities across two industry-standard SIEM platforms: **Splunk** (utilizing Search Processing Language \- SPL) and the **ELK Stack** (Elasticsearch, Logstash, Kibana). You will progress from writing basic single-event queries to designing advanced behavioral-driven correlation logic that tackles real-world threat vectors: Brute Force Attacks, Insider Threats, and Privilege Escalation. Additionally, you will learn how to tune alerts to reduce noise, build custom exclusion whitelists, and design high-visibility dashboards for analysts and executive leadership.

## **Architecture Overview: The Power of SIEM Correlation**

Traditional signature-based detection triggers alerts on isolated, static events. Advanced correlation logic allows the SIEM to identify sophisticated, multi-stage attacks by linking multiple variables across disparate data sources under specific conditional logic:  
`[ Data Source A: Firewall Logs ]  ---> \`  
                                        `\ ---> [ SIEM Correlation Engine ] ---> [ Actionable Alert ]`  
`[ Data Source B: Active Directory ] ---> /      (Time Window, Thresholds,       (Account Disabled /`   
`[ Data Source C: File Server Logs ] --->/       Threat Intel Enrichment)         Host Isolated)`

## **Section 1: Writing Advanced Correlation Rules in Splunk (SPL)**

Splunk uses **Search Processing Language (SPL)** to filter, aggregate, and correlate events.

### **Scenario 1.1: Detecting "Impossible Travel" / Credential Abuse**

Instead of flagging a simple failed login, this rule tracks a single user account successfully authenticating from two distinct geographic locations within an impossible time window (e.g., minutes apart).

#### **Logic Breakdown**

1. **Scope:** Monitor authentication events (index=security or tag=authentication).  
2. **Aggregation:** Group events by user.  
3. **Correlation Variables:** Geographic coordinates/IP ranges and timestamps.  
4. **Condition:** Count distinct locations. If distinct locations \>1 within a 15-minute window, evaluate distance vs. time.

#### **Hands-On Implementation (SPL Query)**

`index=security sourcetype=WinEventLog:Security (EventCode=4624 OR EventCode=4625)`  
`| iplocation src_ip`  
`| stats earliest(_time) as first_login, latest(_time) as last_login, values(Country) as countries, values(src_ip) as source_ips by user`  
`| eval num_countries=mvcount(countries)`  
`| where num_countries > 1`  
`| eval time_difference_seconds = last_login - first_login`  
`| where time_difference_seconds < 900`  
`| table user, countries, source_ips, time_difference_seconds`

#### **Incident Response Action Trigger**

If both conditions are met, Splunk can be configured to execute an automated action:

* Send a high-severity alert to the SOC.  
* Trigger an API call (via Webhook) to your Identity Provider (IdP) to suspend the compromised account automatically.

## **Section 2: Writing Advanced Correlation Rules in the ELK Stack**

The ELK Stack (Elasticsearch, Logstash, and Kibana) relies on **Kibana Query Language (KQL)**, Elasticsearch Query DSL, and ElastAlert (or Kibana's built-in Alerting engine) to evaluate logs inside index patterns.

### **Scenario 2.1: Detecting Suspicious Mass File Access**

This rule detects potential automated data exfiltration or credential harvesting by monitoring users who access an unusually high volume of confidential files within a short timeframe.

#### **Logic Breakdown**

1. **Scope:** File Integrity Monitoring (FIM) or File Server Logs (event.category: file).  
2. **Threshold:** Greater than 50 sensitive documents.  
3. **Time Window:** Less than 10 minutes (600 seconds).  
4. **Context Enrichment:** Correlate file paths with specific key indicators (e.g., filenames containing "password", "credential", "confidential").

#### **Hands-On Implementation (KQL & Rule Engine Logic)**

**Step 1: Define the Index Query in Kibana**  
`event.category : "file" AND event.action : "read" AND file.path : (*credential* OR *password* OR *confidential*)`

**Step 2: Apply Threshold & Time-Window Aggregations**  
In Kibana's Alerting rule wizard, configure the following parameters:

* **Group by:** `user.name`  
* **Trigger Condition:** `When count() IS GREATER THAN 50`  
* **Time Frame:** `FOR THE LAST 10 minutes`

#### **Incident Response Action Trigger**

Once validated, the alert extracts Active Directory metadata, enriches the log with user context, and:

* Routes an interactive notification payload containing user details and affected file paths directly to the incident response team's **Slack** channel.

## **Section 3: Real-World Scenarios & Deployment Logic**

### **Use Case A: Multi-Stage Brute Force Attack Detection**

Attackers bypass basic rate-limiting detection by spreading out authentications or utilizing automated bots to target multiple accounts.

#### **1\. Splunk Implementation**

Detect 20 or more failed logins from a single IP address in under 5 minutes, cross-referenced with a threat intelligence blacklist feed.  
`index=security EventCode=4625`  
`| bucket _time span=5m`  
`| stats count as failed_attempts by src_ip, user`  
`| where failed_attempts >= 20`  
`| lookup threat_intel_ip_list ip_address as src_ip OUTPUT is_malicious`  
`| eval severity = if(is_malicious="true", "Critical", "High")`

* **Auto-remediation:** The SIEM triggers a script on the perimeter firewall to block the offending external source IP temporarily.

#### **2\. ELK Implementation**

Utilize Elasticsearch aggregations to track failing login patterns over time.  
`{`  
  `"query": {`  
    `"bool": {`  
      `"must": [`  
        `{ "term": { "event.category": "authentication" } },`  
        `{ "term": { "event.outcome": "failure" } }`  
      `]`  
    `}`  
  `},`  
  `"aggs": {`  
    `"by_ip": {`  
      `"terms": {`  
        `"field": "source.ip",`  
        `"size": 10`  
      `},`  
      `"aggs": {`  
        `"failures_over_time": {`  
          `"date_histogram": {`  
            `"field": "@timestamp",`  
            `"fixed_interval": "5m"`  
          `}`  
        `}`  
      `}`  
    `}`  
  `}`  
`}`

### **Use Case B: Insider Threat Identification via Behavior Baselining**

Insider threat detection focuses on identifying behavioral deviations against an established baseline rather than static indicators of compromise (IOCs).

#### **Scenario Logic Flow**

`[ Lagos, Nigeria Workspace ] ---> User normally logs in 09:00 - 17:00`  
      `| (Behavior Deviation Detected)`  
      `+---> Logged in from a foreign country at 00:00 (Midnight)`  
      `+---> Massive sensitive data download initiated`  
      `+---> SIEM baseline violation triggers high-severity anomalous event alert`

#### **Splunk Implementation (Anomalous Activity Search)**

`index=security action=success`  
`| stats earliest(_time) as login_time by user, src_country`  
`| eval hour_of_day = date_hour`  
`| where (hour_of_day >= 22 OR hour_of_day <= 4) AND src_country != "NG"`

* **Contextual Response:** Rather than executing a generic account block, this triggers a **conditional access policy** requiring immediate Multi-Factor Authentication (MFA) or suspends access rights temporarily pending analyst verification.

### **Use Case C: Unauthorized Privilege Escalation**

Detect unauthorized modifications to security groups, roles, or user permissions that occur without an approved change ticket.  
`[ Active Directory Security Group Modified ]`   
                 `|`  
                 `v`  
  `[ SIEM Cross-References ITSM System (e.g., Jira/ServiceNow) ]`  
                 `|`  
        `+--------+--------+`  
        `|                 |`  
 `[ Ticket Found ]  [ No Ticket Found ]`  
   `(No Alert)             |`  
                          `v`  
               `[ UNAUTHORIZED PRIVILEGE ESCALATION ]`  
                          `|`  
                          `v`  
               `* Trigger High-Severity Incident Response Action`  
               `* Auto-Revoke Permissions`

#### **Splunk Correlation Query (With Lookup)**

`index=security (EventCode=4728 OR EventCode=4732 OR EventCode=4756)`   
`| rename member_dn as target_user, signature as group_added`  
`| lookup ServiceNow_Change_Tickets change_id as ticket_number OUTPUT status, approved_by, approved_group`  
`| eval is_unauthorized = if(isnull(status) OR status!="Approved", "YES", "NO")`  
`| where is_unauthorized="YES"`  
`| table _time, user, target_user, group_added, is_unauthorized`

## **Section 4: Tuning Alerts and Reducing Noise**

High false-positive rates lead to **alert fatigue**, causing security analysts to overlook genuine compromises. Effective rule design requires precise threshold configuration, whitelisting parameters, and alert suppression.  
       `[ Raw Events Flow ]`  
                `|`  
                `v`  
  `+---------------------------+`  
  `|  Tuning Filter Validation |  <--- [ Exclude Backup Systems, Standard APIs, Trusted Geographies ]`  
  `+---------------------------+`  
                `|`  
                `v`  
     `[ Threshold Evaluation ]    <--- [ Failed Logins > 10 within 10 Minutes ]`  
                `|`  
                `v`  
   `[ Actionable High-Value Alert ]`

### **Scenario 4.1: Implementing Threshold Tuning**

To prevent alert triggers on minor human errors (such as occasional password typos), implement count-to-time constraints.

* **Ineffective Rule:** Trigger an alert on *any* single failed login event.  
* **Tuned Rule:** Trigger an alert only if failed logins exceed a threshold (T \> 10\) within a specific time window (t \= 10 minutes).

#### **Splunk Implementation**

`index=security EventCode=4625`  
`| stats count as failed_attempts by src_ip, user`  
`| where failed_attempts > 10`

#### **ELK Implementation (Kibana Rules Engine)**

* **Threshold Condition:** Group by `user.name` and `source.ip`.  
* **Value:** Set `When count()` to `> 10`.  
* **Time frame:** Set to `10m`.

### **Scenario 4.2: Creating Exception Whitelists (Benign Behavior Suppression)**

Automated systems, such as scheduled enterprise backup utilities, periodically access large amounts of data at night. These anomalous but legitimate processes must be whitelisted to keep dashboards clean.

#### **Splunk White-listing Logic**

Using an external lookup table (trusted\_backups.csv) containing service account names:  
`index=security event_action=read_access`  
`| lookup trusted_backups.csv service_account as user OUTPUT is_trusted_backup`  
`| eval is_trusted_backup = coalesce(is_trusted_backup, "false")`  
`| where is_trusted_backup != "true"`

#### **ELK Stack White-listing Logic**

Utilize KQL queries combined with boolean operators inside the Kibana alerting rule logic:  
`event.category : "file" AND NOT (user.name : "svc_backup" OR user.name : "system_engine")`

## **Section 5: Designing Dashboards for Threat Visibility & Incident Response**

Interactive dashboards translate parsed log structures into operational visual metrics. Clicking a visualization panel should allow immediate **drill-down** to the raw log events for rapid triage.  
`[ Dashboard Panel Visualization ]`  
                `| (Click Panel)`  
                `v`  
   `[ Drill-Down to Underlying Logs ]`  
                `|`  
                `v`  
  `[ Extraction of Indicators (IOCs) ]`

### **Dashboard 1: User Activity Dashboard (Identity Security)**

* **Goal:** Track identity vectors, authentication patterns, and potential credential manipulation.  
* **Key Visualizations:**  
  1. **Failed Logins Map:** Geolocation of all failed authentications (highlighting anomalous external source countries).  
  2. **Top Targets:** A bar chart displaying the usernames with the highest frequency of failed login attempts.  
  3. **Impossible Travel Ledger:** A list showing concurrent logins from geographical boundaries that cannot be physically crossed within the time differential.

#### **Key SPL Visualization Query (Top Failed Users)**

`index=security EventCode=4625`   
`| stats count by user`   
`| sort - count`   
`| head 10`

### **Dashboard 2: Network Anomalies Dashboard (Threat Hunting)**

* **Goal:** Detect lateral movement, unauthorized external connections, data exfiltration, and Command and Control (C2) beacons.  
* **Key Visualizations:**  
  1. **Outbound Data Volume:** An area chart highlighting sudden spikes in outbound transfers, particularly outside business hours.  
  2. **Non-Standard Geographies:** A donut chart representing traffic flow to countries outside your corporate business footprint.  
  3. **Unusual Port Usage:** Traffic tracking across high-risk or uncommon port assignments (e.g., port 4444, 139, 445).

#### **Key KQL Search (Data Exfiltration Spikes)**

`destination.ip : * AND NOT destination.ip : 10.0.0.0/8 AND network.bytes_written > 104857600`

### **Dashboard 3: Endpoint Health & Threat Detection Dashboard**

* **Goal:** Maintain situational awareness of host configurations, system modifications, and execution anomalies.  
* **Key Visualizations:**  
  1. **PowerShell Execution Spikes:** Monitoring processes executing encoded commands (`PowerShell.exe -EncodedCommand`).  
  2. **Unusual Registry Modifications:** Alerts tracking critical keys modified on endpoints (e.g., persistence mechanisms in Run/RunOnce registry hives).  
  3. **Vulnerability State:** Tracking host systems with disabled endpoint agents or missing critical software patches.

#### **Key SPL Query (Encoded PowerShell Execution Tracking)**

`index=security process_name=powershell.exe (args="-encodedcommand" OR args="-e" OR args="-enc")`  
`| table _time, host, user, process_name, args`

### **Dashboard 4: Incident Response Dashboard (SOC Operations)**

* **Goal:** Provide a central triage console ("Mission Control") for active investigations during live threat outbreaks.  
* **Key Visualizations:**  
  1. **Active Incidents Lifecycle:** State columns displaying the status of active alerts (Unassigned, Investigating, Contained, Resolved).  
  2. **Owner Workload Allocation:** Tracking analyst ticket assignment distribution to balance investigation resources.  
  3. **Affected Endpoints Counter:** A dynamic numerical count showing the total number of systems with verified indicators of compromise (IOCs).

### **Dashboard 5: Executive Summary Dashboard (C-Suite Metrics)**

* **Goal:** Translate technical log states into business-oriented operational metrics for executive leadership.  
* **Key Visualizations:**  
  1. **Threat Mitigation Success Rate:** Numerical percentage of detected threats successfully contained automatically or manually by the SOC.  
  2. **Incident Timeline Metrics:**  
     * **Mean Time to Detect (MTTD):** Trend chart showing the duration from initial exploit entry to rule detection.  
     * **Mean Time to Respond (MTTR):** Trend chart showing the duration from detection to complete threat containment.  
  3. **Compliance Posture Tracker:** Visual representation of host compliance levels aligned to regulatory standards (e.g., ISO 27001, PCI-DSS).

   `[ Incident Lifecycle Metrics ]`  
   `+------------------------------------+`  
   `|  MTTD: ~4.5 Minutes (Decreasing)   |`  
   `+------------------------------------+`  
   `|  MTTR: ~12.2 Minutes (Optimized)   |`  
   `+------------------------------------+`

## **Section 6: Unified Defensive Architecture & Roadmap**

Modern threat landscapes require a layered, mature defense strategy. Enterprise security cannot rely on a single detection type.

### **The Multi-Layered Detection Stack**

1. **Signature-Based Detection:** Catching known bad actors, file hashes, and common bad IPs instantly.  
2. **Behavioral-Based Analytics (UBA):** Establishing standard baseline thresholds to identify new, zero-day threat patterns.  
3. **Threat Intelligence Enrichment:** Overlaying external feeds onto internal assets to prioritize severe attacks.  
4. **Log Correlation:** Connecting disparate events across your infrastructure to reconstruct complete attack chains.

                  `[ Enterprise Infrastructure Logs ]`  
                                  `|`  
                                  `v`  
`+-------------------------------------------------------------------+`  
`|                           DETECTION LAYER                         |`  
`|                                                                   |`  
`|  [ Signature Engines ] ---> Flag Known Bad Hashes / Static IOCs   |`  
`|                                                                   |`  
`|  [ Behavioral UBA ]    ---> Flag Anomalous Baseline Deviations    |`  
`|                                                                   |`  
`|  [ Threat Intel Feeds] ---> Prioritize Threats via IP / Domain Rep|`  
`+-------------------------------------------------------------------+`  
                                  `|`  
                                  `v`  
                    `[ Automated SOAR Remediation ]`  
                    `(Isolate Host / Block Firewalls)`

### **Next Steps for Lab Practice**

* **Rule Automation:** Explore Playbook configuration in SOAR tools (Security Orchestration, Automation, and Response) to run scripts directly on firewalls or directory services when high-severity correlation patterns trigger.  
* **Machine Learning Features:** Enable clustering and anomaly-detection capabilities inside your SIEM platforms to auto-update baselines without manual thresholds.  
* **Continuous Testing:** Emulate real attacker techniques (using frameworks like Atomic Red Team) to safely validate that your correlation rules trigger as designed.

2. # **Monitoring Login Anomalies with a SIEM Tool**

This module covers the core concepts and hands-on steps for using Splunk **Transforming Commands**. These commands convert raw search results into organized data structures (such as tables) that are essential for statistical analysis and visual dashboards.  
In this lab, you will start the Splunk service in a Linux environment, upload web server access logs, and run search queries using the `highlight`, `chart`, and `stats` commands.

## **1\. Environment Setup & Starting Splunk**

Before running search queries, you must ensure that your Splunk instance is running on your security workstation (Kali Linux).

### **Step 1: Open the Terminal and Navigate to the Splunk Bin Directory**

1. Open a terminal window in Kali Linux.  
2. Change your working directory to the Splunk installation directory:  
   `cd /opt/splunk/bin`

### **Step 2: Start the Splunk Service**

1. Execute the Splunk startup script with superuser privileges:  
   `sudo ./splunk start`

2. Input your system password when prompted.  
3. Wait for the service to finish loading and setting up the environment.

### **Step 3: Access the Splunk Web Interface**

1. Once Splunk has successfully started, the terminal will display the Splunk Web URL (typically `http://localhost:8000` or the system's IP address on port `8000`).  
2. Copy this URL, open your web browser, and paste it into the address bar.  
3. Log in using your admin credentials.

## **2\. Ingesting Web Application Logs**

You will now add a new log dataset to analyze during this session.

### **Step 1: Access the Add Data Wizard**

* In the Splunk Web interface, click on **Settings** in the top navigation bar and select **Add Data**, or click the **Add Data** icon on the home dashboard.

### **Step 2: Locate and Upload the Access Log**

1. Navigate to the dataset directory you previously downloaded (often distributed as tutorial.zip and unzipped on your workstation).  
2. Inside the unzipped folder, navigate to the www/www1/ directory.  
3. Select and upload the file named **access.log**.

### **Step 3: Configure Input Settings**

1. Click **Next** to proceed to the **Input Settings** screen.  
2. Verify that Splunk automatically recognizes the source type as **access\_combined\_wcookie** (web application logs capturing cookies).  
3. Set the **Host** field value to:  
   `web app`

4. Keep the **Index** set to default (or main).  
5. Review your configuration and click **Submit**.  
6. Once the upload is successful, click on **Start Searching**.

## **3\. Utilizing Transforming Commands**

Now that your logs are ingested, you will execute queries utilizing the three primary transforming commands: highlight, chart, and stats.

### **Task A: The highlight Command**

The highlight command visually emphasizes specific terms inside raw search results. It takes search terms as arguments, separating multiple terms with commas.

1. In the search bar, enter the following query to filter for our web application and highlight occurrences of the browser type **"Safari"**:  
   `host="web app" | highlight Safari`

2. To highlight multiple terms at once (e.g., **"Safari"**, **"buttercup"**, and **"Apple"**), separate them with a comma:  
   `host="web app" | highlight Safari, buttercup, Apple`

3. Observe how these specific strings are highlighted in yellow within the returned log entries, allowing for rapid visual inspection.

### **Task B: The chart Command**

The chart command organizes data into a tabular format, which can then be displayed using graphical charts (such as column, line, bar, or area charts).

#### **Goal: Plot the average data transfer size (in bytes) for each accessed file type.**

1. Enter the following query into the search bar:  
   `host="web app" | chart avg(bytes) by file`

   * *Explanation:*  
     * host="web app" filters the log events to only those originating from the web application server.  
     * chart avg(bytes) by file calculates the average value of the bytes field (representing data transferred) and groups the results by the resource name in the file field.  
2. Run the query and review the resulting statistics table showing each file name (e.g., index.html, cart.do, category.screen) alongside its corresponding average bytes.

### **Task C: The stats Command**

The stats command performs statistical calculations on your search results, grouping them according to specified field values.

#### **Goal: Count the number of file access events that occurred on each day of the week.**

1. Clear the search bar and input the following query:  
   `host="web app" | stats count(file) by date_wday`

   * *Explanation:* This counts occurrences of the file field and groups them by the default weekday field (date\_wday).  
2. Run the search query. The output will display a table with two columns: date\_wday (listing weekdays like Friday, Monday, Saturday, etc.) and count(file) (showing the numerical count of events).

#### **Visualizing the Results:**

1. Below the search bar, switch from the **Statistics** tab to the **Visualization** tab.  
2. By default, Splunk may present the data as a bar chart.  
3. Click on the chart type dropdown menu on the left side of the screen and select **Pie Chart** to see the distribution of file access events across weekdays represented as proportional slices.

### **Summary of Commands Covered**

| Command | Usage | Example Query |
| :---- | :---- | :---- |
| **highlight** | Highlights target strings in raw events | ... | highlight Safari, buttercup |
| **chart** | Produces tabular results suitable for charting | ... | chart avg(bytes) by file |
| **stats** | Generates statistical summaries (counts, averages, etc.) | ... | stats count(file) by date\_wday |

# **Part 2: Reports, Dashboards, and Lookups Setup**

This module covers saving search queries as reusable **Reports**, assembling metrics onto **Dashboards** for non-technical stakeholders, and setting up **Lookups** to map raw technical data into meaningful human-readable values.

## **4\. Working with Splunk Reports**

Reports are saved search configurations. When executed, they run the stored search parameters against the current dataset index to display up-to-date statistical and visual representations.

### **Step 1: Save a Search Query as a Report**

1. Ensure your active search query is:  
   `host="web app" | stats count(file) by date_wday`

2. Locate the **Save As** dropdown button above the search bar.  
3. Select **Report** from the menu.  
4. Fill in the Report metadata:  
   * **Title**: `my weekday report`  
   * **Description**: `This is my weekday report` (Optional)  
5. Click **Save** and then click **View** to inspect your new report.

### **Step 2: Editing Report Settings**

While viewing the report, you can manage permissions, schedules, and queries:

1. Click **Edit** to display configuration options:  
   * **Edit Description**: Modify report summary.  
   * **Edit Permissions**: Share the report with other users or roles in your organization.  
   * **Edit Schedule**: Automate search intervals (detailed in Part 3).

### **Step 3: Modifying Stored Queries ("Open in Search")**

If you need to change the underlying logic of a saved report without creating a new one:

1. Click the **Edit** button on your report.  
2. Select **Open in Search**. This opens the exact stored query in the standard Search window.  
3. Modify the query as needed and re-save the changes.

## **5\. Creating and Managing Dashboards**

Dashboards group visual panels (tables, charts, single values) to present a high-level overview of system metrics, catering to executives, directors, or managers who do not require technical command execution.

### **Step 1: Save a Panel to a New Dashboard**

1. Run a search query (such as your weekday count query) and navigate to the **Save As** dropdown.  
2. Select **Dashboard Panel**.  
3. In the panel creation wizard, configure the following:  
   * **Dashboard**: Select **New**.  
   * **Dashboard Title**: `my weekday dashboard`  
   * **Dashboard Description**: `This is my weekday dashboard`  
   * **Dashboard UI Option**: Select **Classic Dashboards** (instead of Dashboard Studio).  
   * **Panel Title**: `my lovely weekday dashboard`  
4. Click **Save to Dashboard**, then click **View Dashboard**.

### **Step 2: Exporting Dashboards to PDF**

Dashboards can be exported as PDF files for off-platform sharing:

1. Open your dashboard view.  
2. Click **Export** in the top menu.  
3. Select **Export PDF**. The system will download a cleanly formatted PDF of your dashboard visualizations.

### **Step 3: Scheduling Automatic Email Delivery**

Splunk can generate and email your dashboard panels on a recurring schedule:

1. Navigate to your dashboard, click **Export**, and select **Schedule PDF Delivery**.  
2. Check the box to enable scheduled delivery.  
3. Configure the schedule:  
   * **Schedule**: Set to **Run weekly**.  
   * **Day & Time**: Select **Monday** at **9:00 AM** (e.g., to review metrics one hour prior to a 10:00 AM weekly operations meeting).  
   * **Email To**: Input the target address, for example: victor@outcoafrica.com.  
   * **Priority**: Keep as **Normal** (or adjust to **Highest** to prioritize this execution over other simultaneous schedules).  
4. Click **Save**.

## **6\. Creating and Configuring Lookups**

Lookups allow you to enrich your event data by matching fields against external data sources (like a CSV reference table). For example, rather than displaying an abstract product ID number, lookups let Splunk match that ID against a CSV reference table to print the actual product name in your results.

### **Step 1: Create the Reference CSV on the Workstation**

You will generate a lookup database locally on your workstation first.

1. Open a new terminal instance in Kali Linux.  
2. Navigate to your Downloads folder:  
   `cd ~/Downloads`

3. Create and move into a new directory dedicated to lookup files:  
   `mkdir lookup_file`  
   `cd lookup_file`

4. Create a file named product\_id\_value.csv using the nano text editor:  
   `nano product_id_value.csv`

5. Paste your reference data into the editor. It should look like this (mapping product IDs to user-friendly descriptions):  
   `productId,productName`  
   `HPC-991,Hard Drive`  
   `SFT-882,Software License`  
   `RAM-002,DDR4 RAM`

   *(Note: Alternatively, if you already have a product.csv file inside your downloaded exercises directory, you can locate it, verify its content using cat \<filename\>, copy the selection, and paste it into your new lookup file).*  
6. Save and exit nano (Ctrl+X, then press Y and Enter).

### **Step 2: Upload the Lookup File to Splunk**

1. In the Splunk Web interface, click **Settings** and select **Lookups**.  
2. Click **Lookup table files**, then click **New** (or **Add New**).  
3. Select the target **Destination App** (leave as default/search).  
4. Click **Browse** and select your locally created file: \~/Downloads/lookup\_file/product\_id\_value.csv.  
5. Name the uploaded file on the Splunk system:  
   `product_id_value.csv`

6. Click **Save**.

### **Step 3: Define the Lookup Configuration**

A Lookup Definition maps the reference file so that search queries can utilize it.

1. Go back to **Settings** \-\> **Lookups**.  
2. Click on **Lookup definitions**, then click **Add New**.  
3. Configure the Lookup Definition parameters:  
   * **Name**: product\_id\_description (or new\_product\_id\_description if modifying / distinguishing versions)  
   * **Type**: Select **File-based**  
   * **Lookup file**: Select **product\_id\_value.csv** from the dropdown list.  
4. Click **Save**.  
5. Verify your lookup definition appears in the definitions list (you can search for "product" in the search box to find it).

# **Part 3: Lookups in Search & Report Scheduling**

This module covers running searches that utilize our newly defined lookups, customizing visualization options for lookup results, and deep-diving into the options available when scheduling reports.

## **7\. Enriching Search Queries with Lookups**

Now that both the lookup CSV file and the lookup definition are successfully configured in Splunk, you can execute search queries that automatically map raw product IDs within the web application traffic logs to human-readable product names.

### **Step 1: Navigate to the Search & Reporting App**

1. From the Splunk home dashboard or the left side menu, click on **Search & Reporting**.  
2. Clear any active search query in the search bar.

### **Step 2: Run a Lookup-Enriched Query**

Enter and execute the following search query to map product IDs and summarize access frequency:  
`host="web app" | lookup product_id_description productId OUTPUT productName | stats count by productName`

* **Explanation of Query Structure:**  
  * host="web app": Restricts results to logs generated by our web application server.  
  * | lookup product\_id\_description: Calls the lookup definition we created in Part 2\.  
  * productId: Tells Splunk to use the existing productId field inside the logs as the key to map against the lookup file.  
  * OUTPUT productName: Appends the matching value from the productName column in the lookup CSV as a new field on the active results.  
  * | stats count by productName: Performs a statistical count of events, grouping the totals by the newly resolved, human-readable product name.

### **Step 3: Handle Empty Results and Time Frame Calibrations**

1. If your query returns "No results found," verify your active search time range.  
2. In the dropdown menu on the right of the search bar, change the timeframe from **Presets** to **All Time** (or standard presets like **Last 24 hours** or **Last 7 days** depending on the timestamp of your access logs).  
3. Run the search query again. You will see a clean statistics table displaying real product titles (such as *Hard Drive*, *Software License*, etc.) instead of obscure numeric IDs.

### **Step 4: Alternate Visualizations**

To make these lookup summaries easier to interpret, experiment with the visualization options:

1. Switch to the **Visualization** tab below the search bar.  
2. Select **Pie Chart** to view the proportional distribution of products accessed.  
3. Switch visualization options to **Bar Chart** or **Line Chart** to compare volumes dynamically.

## **8\. Deep Dive: Report Scheduling Features**

Scheduling automates report execution without requiring manual input. This is critical for generating routine reports, improving dashboard load times (by pre-calculating results in the background), and automating system updates.

### **Step 1: Accessing the Schedule Editor**

1. In the top navigation bar, select **Schedules** or go to **Reports**, locate your saved report (e.g., my weekday report), click **Edit**, and select **Edit Schedule**.  
2. Tick the checkbox to **Schedule Report**.

### **Step 2: Understanding Scheduling Configurations**

Splunk provides granular parameters to control how, when, and with what priority scheduled searches run.

#### **1\. Time Range**

* **Purpose:** Sets the lookback time window for the report.  
* **Options:** You can run a schedule over **All Time**, the **Last 24 hours**, the **Last 15 minutes**, or a custom window. For optimal server performance, restrict this to the smallest window necessary to capture the target event cycle.

#### **2\. Schedule Priority**

* **Purpose:** Prevents resource contention. If multiple reports are scheduled to trigger at the exact same moment, Splunk uses priority to queue them.  
* **Options:** Select from **Lowest**, **Low**, **Normal**, **High**, or **Highest**. Business-critical reporting (like login security reviews) should receive a higher priority than routine analytical reports.

#### **3\. Schedule Window**

* **Purpose:** Mitigates CPU spikes on Splunk search heads. When several reports share the same priority level, a schedule window allows Splunk to run the report within a flexible time range rather than forcing all of them to start simultaneously.  
* **Options:** Select **No Window**, **5 minutes**, **15 minutes**, or **30 minutes**. Setting a 5-minute window means Splunk will distribute the execution start time dynamically within 5 minutes of the scheduled trigger time.

## **9\. Configuring Trigger Actions and Webhooks**

After a scheduled report finishes execution, Splunk can perform automated downstream actions called **Trigger Actions**.

### **Step 1: Open Trigger Actions**

* Inside the **Edit Schedule** menu, scroll down to the **Trigger Actions** section.  
* Click **Add Action** to display the dropdown of available automations.

### **Step 2: Understanding Action Types**

| Action Type | Operational Use Case |
| :---- | :---- |
| **Send Email** | Delivers report results or PDF attachments directly to stakeholders. |
| **Log Event** | Sends a custom log event indicating report execution status back to a Splunk index or custom receiver endpoint. |
| **Output Results to Lookup** | Overwrites or appends the search output directly into a CSV lookup file to build dynamic reference lists. |
| **Run a Script** | Executes a local python or shell script on the Splunk host machine for advanced custom processing. |
| **Webhook** | Sends an HTTP POST payload to a external URL, bridging Splunk with third-party software (like Slack, Microsoft Teams, or custom APIs). |

### **Step 3: Setting Up a Webhook**

Webhooks allow Splunk to interact seamlessly with external infrastructure.

1. Select **Webhook** from the **Add Action** dropdown menu.  
2. In the configuration window, input the **Webhook URL** provided by your external upstream application.  
3. *How it operates:* When the scheduled search parameters are evaluated, Splunk issues an HTTP POST request containing JSON-formatted metadata of the search results to the designated endpoint.  
4. *Fail-safe mechanics:* Splunk’s webhook subsystem will attempt to transmit the POST request. Based on client-side configurations, if the upstream server fails to reply with an HTTP 200 OK response, Splunk can retry the request at structured intervals (e.g., every 5 minutes). If the server remains unreachable after a pre-determined retry threshold (typically 10 attempts), Splunk halts execution and logs a transport error.

# **Part 4: Alerts, Knowledge Management & Subsearching**

This final module covers creating conditional **Alerts** based on live query results, understanding enterprise **Knowledge Management** object types, and constructing **Subsearches** to evaluate nested data loops.

## **10\. Creating and Configuring Alerts**

Alerts in Splunk are automated actions triggered when specific live dataset criteria are met. Unlike reports, which run on predefined intervals to capture summaries, alerts are designed for immediate incident detection (such as brute force security events).

### **Step 1: Open a Query in Search**

1. Ensure you have an active query running in the **Search & Reporting** app (e.g., your lookup query or weekday report query).  
2. Click **Save As** in the top menu above the search bar and select **Alert**.

### **Step 2: Configure Alert Metadata & Permissions**

1. In the creation wizard, fill out the basic metadata:  
   * **Title**: my weekday alert  
   * **Description**: This is my weekday alert  
2. Configure **Permissions**:  
   * **Private**: Keeps the alert private to the creator (only you can run, view, or edit it). Select **Private** for this lab.  
   * **Shared in App**: Shares the alert with other app users. Standard users receive read-only permissions, while Power Users retain edit access.

### **Step 3: Configure Alert Type, Trigger Conditions & Actions**

1. Set the **Alert Type**:  
   * **Scheduled**: Runs at fixed, predetermined intervals.  
   * **Real-time**: Continuously monitors incoming data streams. Select **Real-time** (set to expire in 24 hours).  
2. Define the **Trigger Condition** (the evaluation rule that determines when to fire the alert):  
   * Select **Number of Results** from the dropdown.  
   * Configure the condition thresholds:  
     `greater than 10`  
     `in 15 minutes`

     *(Operational Security Note: For example, tracking if brute force login attempts from a unique source address are greater than 10 within a 15-minute timeframe).*  
   * Set trigger frequency to **Once** (runs once when the threshold is crossed) or **For each result** (fires for every matching item within the result set).  
3. Set the **Trigger Action**:  
   * Click **Add Action** and select **Log Event**.  
   * Specify the customized event description text inside the **Event** text box to log the alert trigger event back to a target index (e.g., main or custom security index) for auditing.  
   * Click **Save**.

## **11\. Splunk Knowledge Management**

Knowledge objects are configurations in Splunk that normalize data, establish structural context over unstructured logs, and make information collaborative and searchable.

### **Key Classifications of Knowledge Objects**

| Object Type | Description | Operational Use Case |
| :---- | :---- | :---- |
| **Fields & Extractions** | The baseline layer of log normalization. Translates raw, unparsed strings into indexed fields. | Manually extracting a unique session cookie ID from custom web requests. |
| **Event Types & Transactions** | Groups conceptually linked events. Event types group search criteria results; transactions trace related events across time. | Tracing a specific web transaction lifecycle across sequential actions (e.g., shopping cart checkout process). |
| **Lookups & Workflow Actions** | Enrichment tools. Lookups map field IDs to external files; Workflow Actions enable external API or URL interactions from within search results. | Setting up a click-through workflow action to perform a whois lookup on a source IP address field in security alerts. |
| **Tags & Aliases** | Normalizes naming conventions. Aliases reconcile different source system naming discrepancies; tags label collections of values. | Mapping varied fields like clientip and source\_ip to a uniform alias: ip\_address. Tagging servers with location labels like US\_West\_Datacenter. |
| **Data Models & Pivot** | Hierarchical data definitions used to power the Splunk Pivot interface. | Allows non-technical business analysts to visually generate custom tables and visual charts without writing raw SPL. |

## **12\. Utilizing Subsearches in Splunk**

A **Subsearch** (or subquery) is an inner search nested within a primary outer search. The inner query executes first, and its output is immediately passed to the outer query as a filtering argument.

* **Key Syntax Rule:** In Splunk, subsearches **must** be enclosed in square brackets: \[ ... \] (the open/close brackets act as search head boundaries).

### **Goal: Identify all web traffic events occurring on "Sunday" that have a transfer size (bytes) exactly equal to the maximum byte transfer size found across the entire dataset.**

### **Step 1: Draft the Inner Query (Subsearch)**

First, write a query that calculates the absolute maximum data size in bytes across all web logs:  
`host="web app" | stats max(bytes) as bytes`

*(This returns a single field and value: bytes=XXXX).*

### **Step 2: Construct the Complete Nested Search Query**

Next, nest that inner query inside the main outer query, which restricts results to events occurring on Sunday:  
`host="web app" date_wday=Sunday [ search host="web app" | stats max(bytes) as bytes ]`

* **How it evaluates:**  
  1. Splunk executes the subsearch in square brackets first: \[ search host="web app" | stats max(bytes) as bytes \].  
  2. The subsearch resolves the maximum transfer value (e.g., 4521).  
  3. The value is passed to the outer search, which evaluates the final query as: host="web app" date\_wday=Sunday bytes=4521.

### **Step 3: Run the Subsearch**

1. Enter the full combined query in the search bar.  
2. If no results display immediately, verify that your timeframe dropdown is set to **All Time**.  
3. Run the search and observe the results. It will output only Sunday events that match the exact maximum byte size across the index.

