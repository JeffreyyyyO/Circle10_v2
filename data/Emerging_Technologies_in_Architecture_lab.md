# **CYS 530—WEEK 4—LAB**

## **EMERGING TECHNOLOGIES IN ARCHITECTURE**

# **Deploying an AI-Powered Anomaly Detection Using Splunk**

## **Lab Module 1: Splunk Enterprise Installation and Environment Setup**

### **Lab Overview**

This hands-on lab guides you through the complete installation, initialization, and initial configuration of Splunk Enterprise on a Kali Linux environment. You will learn how to bypass standard business registration blocks for lab settings, extract and configure Splunk under secure directories, manage Splunk services using the Command Line Interface (CLI), and navigate the administrative settings of the Splunk Web UI.

### **Objectives**

* Understand Splunk’s core big data capabilities (ingestion, indexing, searching, and modeling).  
* Register and acquire the Linux .tgz installer package for Splunk Enterprise.  
* Perform a secure local installation under the /opt directory to avoid root privilege conflicts.  
* Administer Splunk via the CLI using standard controls (start, stop, restart, help).  
* Configure administrative settings, profile preferences, and default landing pathways in the Web console.

### **Task 1: Registering and Downloading Splunk Enterprise**

Splunk Enterprise offers a fully-featured, 60-day free evaluation. Standard registration requires a business email address. In local lab scenarios, you can bypass this utilizing a temporary disposable email system.

1. Open a web browser in your Kali Linux VM.  
2. Navigate to a disposable temporary mail provider (e.g., temp-mail.org). Copy the generated temporary email address.  
3. Access the official Splunk registration page for the Splunk Enterprise Free Trial.  
4. Fill out the registration form:  
   * **Business Email:** Paste your temporary email address.  
   * **Job Title:** Cybersecurity Engineer (or equivalent).  
   * **Company:** *Your Lab/School Name*.  
   * **Country/Zip Code:** Enter your regional details.  
5. Submit the form and monitor your temporary email inbox.  
6. Open the verification email sent by Splunk and confirm your account.  
7. Once verified, navigate to the **Linux Downloads** tab:  
   * Locate the .tgz (tarball) option for 64-bit systems.  
   * Click **Download Now** to download it to your local filesystem (default: \~/Downloads).

### **Task 2: CLI Installation and Extraction (Kali Linux)**

Extracting Splunk to the /opt directory is a security and operational best practice. This avoids configuration conflicts and mitigates risks associated with running applications directly from the root namespace.

1. Open your Kali terminal.  
2. Navigate to your Downloads folder:rfc  
   `cd ~/Downloads`

3. List the contents to verify the download package exists:  
   `ls`

4. Run the extraction command to unpack Splunk into the /opt directory using superuser privileges:  
   `sudo tar -xvzf splunk-<version>-linux-2.6-amd64.tgz -C /opt`

   *(Note: Replace \<version\> with your specific downloaded version string, or use tab completion.)*

### **Task 3: Managing Splunk Services via CLI**

Splunk binaries reside in /opt/splunk/bin. You must perform administrative tasks inside this directory.

1. Navigate to the Splunk binary pathway:  
   `cd /opt/splunk/bin`

2. Verify the contents to identify the primary splunk execution wrapper:  
   `ls`

#### **Core Service Commands**

Splunk relies on four primary CLI arguments:

* start — Launches background daemons and starts the web interface.  
* stop — Gracefully terminates running Splunk processes.  
* restart — Restarts services (required for application changes and major config updates).  
* help — Prints commands, usage parameters, and configuration guidelines.  
3. Test the help flag using your executable wrapper:  
   `./splunk help`

   * *Tip:* Press the **Spacebar** to advance through the license agreements and help documentation page-by-page (much faster than hitting Enter for line-by-line scrolling).  
4. Initialize the Splunk service, automatically accepting the software license agreement:  
   `sudo ./splunk start --accept-license`

5. When prompted by the installation script, configure your initial boot administrator credentials:  
   * **Administrator Username:** admin  
   * **Password:** \[Create a strong password\]  
   * **Confirm Password:** \[Re-enter your password\]  
6. Wait for the initialization processes to finish. Splunk will launch its web server and output the access URL (default: http://localhost:8000 or http://127.0.0.1:8000).

### **Task 4: Troubleshooting System Environment Conflicts (Optional)**

If you encounter a runtime error indicating Splunk cannot determine the pathing:  
`"Warning: Cannot determine Splunk Home..."`

This typically occurs if a previous installation left stale environmental configuration traces. Run the path export command to point your environmental variables directly to the core installation directory:  
`export SPLUNK_HOME=/opt/splunk`

Re-attempt running the start command afterwards.

### **Task 5: Web Console Administration and Profile Customization**

With the background services operational, configuration shifts to the graphical user interface (GUI).

1. Open your web browser and navigate to:  
   `http://localhost:8000`

2. Authenticate using the local administrative credentials created in **Task 3** (Username: admin).  
3. Dismiss any onboarding notifications to view the main Splunk Enterprise Dashboard.

#### **Modifying Account Details**

1. Click the **Administrator** dropdown profile button in the top navigation panel.  
2. Select **Account Settings**.  
3. Update the **Full Name** field to your name (e.g., Victor).  
4. (Optional) Set up or change password credentials if needed, then click **Save**.

#### **Setting System and UI Preferences**

1. Select **Preferences** under the administrator profile menu.  
2. In the **Global** tab, customize these settings:  
   * **Time Zone:** Select your local lab time zone (critical for matching security log timelines).  
   * **Default Application:** Select **Search & Reporting**. (This forces Splunk to open directly into your main search pane upon login).  
   * **Theme:** Toggle between the default light theme or dark theme depending on your visual preference.  
   * **Restart Background Jobs:** Keep this flag monitored. Enabling this automatically restarts search queries running in the background when the main Splunk daemon is restarted.  
3. Click **Apply** to save preferences.

### **Task 6: Exploring the Administrative Layout**

Familiarize yourself with the core navigational components of Splunk:

| `Splunk Enterprise Bar (Messages, Settings, Activity)` |  |
| ----- | :---- |
| `Apps Panel Search Gateway Analytics` | `Main Workspace / Recommended Actions Add Data Browse Apps Search Documentation` |

1. **Messages Log:** Located in the top bar. Inspect this panel to view real-time system alerts, licensing limitations, security notices, and boot logs.  
2. **Activity Panel:** Click **Activity** \-\> **Jobs** to monitor executing system scripts, background services, and resource-heavy active searches.  
3. **Settings Dashboard:** Navigate to **Settings** to see the five core areas of Splunk administration:  
   * **Knowledge:** Control search parameters, event types, field extractions, and data lookups.  
   * **Data:** Manage file inputs, forwarding rules, index directories, and custom source-type parameters.  
   * **System:** Configure server groupings, system licenses, and optimization patterns.  
   * **Distributed Environment:** Setup search-head clusters and scale indexer topologies.  
   * **Users and Authentication:** Establish Role-Based Access Controls (RBAC), multi-tenant credentials, and LDAP integrations.  
4. **Data Input Pane:** Navigate to **Settings** \-\> **Data Inputs**. Look at the local input configurations (files, directories, scripts, network ports like TCP/UDP) and note where forwarders send standard remote system metrics.  
5. **App Store Integration:** Click **Find More Apps** under the App panel to view available application integrations (e.g., Cisco Security Suite, Windows Active Directory monitors, and Threat Intelligence pipelines) that normalize third-party security data.

### **Task Verification**

* Splunk Enterprise is installed, and the CLI binary wrapper is successfully configured under /opt/splunk.  
* The server daemon is successfully running and binding to network port 8000\.  
* The admin panel is configured, custom timezone metrics are applied, and Search & Reporting is set as the default landing dashboard.

## **Lab Module 2: Security Dataset Acquisition and CLI Data Management**

### **Lab Overview**

This hands-on lab outlines the process of procuring, organizing, and preparing security event datasets for analysis. You will download realistic machine-generated log archives and structured business tables, create clean directory structures within a Kali Linux environment via the CLI, move archives into target namespaces, and safely extract the components in preparation for ingestion.

### **Objectives**

* Source realistic cyber security event logs and transactional structured data from public endpoints.  
* Implement professional CLI file management patterns to isolate development workspaces.  
* Unzip multi-format datasets (.zip formats) using Linux-native utility parameters.  
* Inspect extracted paths to locate key security artifacts (such as secure.log).

### **Task 1: Sourcing and Downloading Security Datasets**

For testing and verification, analysts utilize mock data mimicking production environments. We will download two primary datasets:

1. **tutorialdata.zip**: A compressed archive housing multi-source system, mail, security, and web access logs.  
2. **prices.csv.zip**: A compressed table containing structured database pricing metrics used for cross-reference lookups.  
3. Open your browser in Kali Linux.  
4. Download the datasets from your learning platform's resource section, or source the official public Splunk training resources.  
5. Verify that the files are saved to your local downloads directory (typically \~/Downloads).

### **Task 2: Creating a Workspace and Organizing Datasets via CLI**

To maintain environment cleanliness and prevent asset scattering, you will create a dedicated workspace subdirectory and transfer the downloaded zip files.

1. Open your terminal shell.  
2. Navigate to your downloads directory:  
   `cd ~/Downloads`

3. Create a dedicated workspace directory named splunk2 (or similar unique identifier):  
   `mkdir splunk2`

4. Move the two compressed datasets into your newly created directory. We execute these operations sequentially to prevent parameter binding errors:  
   `mv tutorialdata.zip splunk2/`  
   `mv prices.csv.zip splunk2/`

5. Navigate into your workspace directory and confirm the presence of both archives:  
   cd splunk2  
   `ls -la`

### **Task 3: Command Line Dataset Extraction**

You must extract the nested log structures to expose the raw file formats (such as .csv and flat .log files).

1. Execute the native Linux unzip utility on both compressed archives:  
   `unzip prices.csv.zip`  
   `unzip tutorialdata.zip`

2. Verify the extraction output using the list command:  
   `ls -la`

   *(Note: You will see a structured layout of directories representing different operational divisions such as mail logs, database logs, and server transactions.)*  
3. Navigate into the mail logs subdirectory to verify file exposure:  
   `cd mail_csv`  
   `ls`

   * *Verification:* Confirm the presence of the secure.log file. This log tracks authorization attempts, SSH sessions, and user privilege adjustments, which will serve as our primary dataset for monitoring system health and identifying malicious activities.

### **Task Verification**

* A secure workspace subfolder (splunk2) is initialized inside the host system.  
* Raw datasets are successfully migrated, and extraction outputs map cleanly to distinct source-type directories.  
* The presence of secure.log is verified under your workspace paths.

## **Lab Module 3: Security Data Ingestion and Search Processing Language (SPL) Operations**

### **Lab Overview**

This hands-on lab covers importing your extracted security datasets into Splunk, configuring custom source types, applying structured host classifications, and choosing storage indexes. You will transition to active data query operations by mastering Search Processing Language (SPL) syntax—including metadata exploration, logical boolean combinations, wildcard patterns, string-based refinements, custom field extraction, and advanced temporal boundaries.

### **Objectives**

* Ingest unstructured log files (secure.log) and structured reference sheets (prices.csv) into Splunk.  
* Distinguish the functional use cases for index structures (main, history, summary) and host mapping techniques.  
* Run administrative diagnostic commands to evaluate ingest targets via SPL.  
* Implement logical search queries using wildcard parameters and boolean operators (AND, OR).  
* Filter logs based on granular field values and absolute historical ranges (earliest, latest).

### **Task 1: Ingesting Unstructured Security Logs (secure.log)**

Unstructured data must be brought into Splunk through the data ingestion pipeline. Splunk features built-in "Source Type Detection" engine mechanics to parse files automatically.

1. Log in to your Splunk Web console (http://localhost:8000).  
2. On the default landing dashboard, navigate to **Splunk Recommended** and select **Add Data** (alternatively, click **Settings** \-\> **Add Data**).  
3. Select **Upload** to ingest data from your local computer.  
4. Click **Select File** and browse to your workspace path:  
   `~/Downloads/splunk2/mail_csv/secure.log`

5. Select the file and click **Open**. Once the screen shows a successful validation message, click **Next**.  
6. On the **Set Source Type** window, inspect the preview pane. Observe that Splunk organizes the logs sequentially by parsing timestamps. Keep the **Source Type** selection as **Default** (Splunk will dynamically recognize line breaks) and click **Next**.

### **Task 2: Configuring Host Classifications and Storage Indexes**

During ingestion, you must tell Splunk where the data originated (by assigning a host value) and where it should be stored (by choosing an index).

1. On the **Input Settings** screen, locate the **Host** configuration section.  
2. Observe the three core methodologies for hostname assignments:  
   * **Constant Value:** Applies a static, manual string representing the origin device. (Use this for local lab servers).  
   * **Regular Expression on Path:** Leverages custom regex patterns to capture host identities dynamically from the path directory structure.  
   * **Segment in Path:** Extracts hostnames based on path directories. For instance, in /var/log/server1/app.log, selecting segment 3 pulls server1.  
3. Set your Host field configuration option to **Constant Value**, and input:  
   `secure_log`

4. Locate the **Index** configuration section. Observe the primary database index spaces:  
   * **Main (Default):** The standard, high-speed storage bucket containing search index payloads.  
   * **Summary:** Houses pre-calculated aggregates (sums, counts, averages) to accelerate long-running dashboard reports.  
   * **History:** Dedicated database tracking console activities, diagnostic commands, and user query histories.  
5. Set the index destination to **default** (this maps events directly to main).  
6. Click **Review** to verify your data configurations:  
   * **Input Type:** Uploaded File  
   * **File Name:** secure.log  
   * **Source Type:** secure\_log (Custom/Default)  
   * **Host:** secure\_log  
   * **Index:** default  
7. Click **Submit**. Once the console confirms completion, do not click search yet. Proceed to Task 3\.

### **Task 3: Ingesting Structured Reference Data (prices.csv)**

Structured datasets like CSV files are highly organized, relational collections. Splunk automatically adjusts its parsing engines when structured data is uploaded.

1. Navigate back to **Settings** \-\> **Add Data** \-\> **Upload**.  
2. Click **Select File** and choose the pricing database file:  
   `~/Downloads/splunk2/prices.csv`

3. Upload the file and click **Next**.  
4. Observe the **Source Type** window. Because the file contains comma-separated values, Splunk's parsing engine automatically applies the csv source type.  
5. Review the preview pane. Note that Splunk renders the raw rows as an interactive columned table with headers (code, price, product\_name, sale\_price, timestamp). Click **Next**.  
6. Set the **Host** field option to **Constant Value** and input:  
   `prices`

7. Retain the target **Index** as **default** and click **Next** \-\> **Review** \-\> **Submit**.  
8. Click **Start Searching** to open the Search & Reporting app window.

### **Task 4: Querying System Metadata and Sources**

The Search & Reporting app uses Search Processing Language (SPL). You will use administrative commands to verify your data streams.

1. Ensure the search window time range (located to the right of the search bar) is set to **All Time** or **Last 30 Days**.  
2. Clear the search bar, type the following generating metadata query, and press Enter:  
   `| metadata type=sources`

   *(Note: The leading pipe symbol | must be included. This tells Splunk to generate a report from database indexes rather than running a raw string filter.)*  
3. Verify that the table output successfully lists both your ingested sources:  
   * .../secure.log  
   * .../prices.csv  
4. Run a direct search query to examine the contents of the pricing table:  
   `source="*prices.csv"`

5. Observe the database output. Note how Splunk displays individual structured rows as discrete events.

### **Task 5: Master Wildcards and Logical Operators**

SPL supports pattern-matching wildcards (\*) and boolean logic (AND, OR, NOT). *Note: Boolean operators must always be written in uppercase.*

1. Run a pattern-matching wildcard query on the pricing data to retrieve all product attributes starting with "price":  
   `price*`

2. Run a wildcard search on the security data to capture all SSH events:  
   `ssh*`

   Observe that this retrieves records containing sshd and its operational threads.  
3. Run a query that combines pricing variables and security daemon parameters using an AND operator:  
   `price* AND sshd*`

   * **Observation:** The search returns **0 results**. This occurs because no single log event contains both a pricing transaction variable and an SSH operating system daemon event.  
4. Modify the query to use the OR logical operator:  
   `price* OR sshd*`

   * **Observation:** The search returns thousands of events. Splunk successfully combines the data streams, presenting log results from both files in a unified timelines graph.

### **Task 6: Custom Field Selections, Refinement, and Temporal Filtering**

Refining searches allows analysts to drill down into anomalies by isolating specific security fields, tracking specific process IDs, and scoping query timelines.

1. Search for failed login events across your security index:  
   `fail* AND password`

2. Examine the events. Locate a process ID or tracking number (e.g., Process ID 3351).  
3. To isolate that specific thread, double-click on the string 3351 in the event interface and select **Add to Search**, or append it manually:  
   `fail* AND password 3351`

   Observe that the search updates to display only events associated with that specific execution process.

#### **Configuring the Fields Panel**

1. Locate the **Fields Panel** on the left side of the search workspace.  
2. Review the **Selected Fields** list (typically defaults to host, source, and sourcetype).  
3. Scroll down to **Interesting Fields**. Locate the temporal indicators:  
   * date\_hour  
   * date\_mday (Day of month)  
   * date\_minute  
   * date\_second  
   * date\_month  
4. Click on these fields and select **Yes** to add them to your Selected Fields. This locks them to your sidebar panel for rapid metric monitoring.

#### **Querying Custom Field Values**

1. Run an SPL query to locate password failures specifically on the 15th day of the month:  
   `fail* AND password host=secure_log date_mday=15`

2. Modify the query parameters to view password failures on the 16th day:  
   `fail* AND password host=secure_log date_mday=16`

#### **Setting Boundary Rules with Time Modifiers**

While the graphical dropdown menu provides time-range presets (e.g., Last 15 minutes, Last 24 hours, Last 30 days), security analysts often enforce temporal boundaries directly in their search queries.

1. Clear the search bar and write the following query to isolate host events within a strict, relative chronological window:  
   `host=secure_log earliest=-15d latest=-7d`

   *(Explanation: This restricts search results to events that occurred between exactly 15 days ago and 7 days ago, ensuring precise windowing for security incidents.)*

### **Task Verification**

* Raw datasets (`secure.log` and `prices.csv`) are successfully ingested into the default index directory.  
* Auto-detection parameters successfully map files to structured (`csv`) and unstructured (`syslog style`) categories.  
* System diagnostics confirm active source directories via the command `| metadata type=sources`.  
* Search filters demonstrate proper pattern isolation using logical arguments (`AND`, `OR`), wildcard characters (`*`), explicit field definitions (`date_mday`), and temporal constraints (`earliest, latest`).