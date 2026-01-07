# **CYS 533—WEEK 1**

## **THREAT DETECTION, COMPLIANCE AND HARDENING IN THE CLOUD**

1. # **Introduction to Threat Detection, Compliance, and Hardening in the Cloud**

## **Overview**

Cloud security is a critical and rapidly evolving area within cybersecurity. As organizations migrate infrastructure and services to major platforms—including AWS, Microsoft Azure, and Google Cloud Platform (GCP)—it is essential to understand the unique security challenges presented by these environments.

## **Learning Objectives**

By the end of this module, you will be able to:

* **Identify Cloud-Specific Threats:** Understand how these threats manifest in real-world scenarios and news.  
* **Compliance and Audit Readiness:** Define what it means to be compliant and prepared for audits within a cloud environment.  
* **Cloud-Native Logging and Monitoring:** Effectively leverage native tools to maintain visibility and security.  
* **Security Hardening Techniques:** Apply effective hardening strategies using native cloud features.

2. # **Compliance and Audit Readiness in the Cloud**

## **Overview**

As small businesses migrate to the cloud, compliance and audit readiness have become critical components of cybersecurity.

* **Cybersecurity Compliance:** Ensuring systems and practices meet established rules, laws, or standards defined by governments, industry groups, or regulators to protect sensitive or personal data.  
* **Audit Readiness:** Having the necessary evidence (documentation, logs, access controls, configurations) in place for an external auditor to verify that required cloud standards are being met.  
* **Importance:** Organizations in finance, healthcare, education, and government expect cybersecurity teams to understand and maintain compliance.

## **1\. SOC 2 (System and Organization Controls Type 2\)**

SOC 2 is a primary compliance standard for companies handling customer data through internet or cloud services. Developed by the AICPA, it assesses how well a company protects data and systems to build customer trust.

### **Five Trust Service Principles**

1. **Security:** Protecting systems against unauthorized access using firewalls, intrusion detection, IAM, and encryption (e.g., using role-based access control for production servers).  
2. **Availability:** Ensuring systems are operational as needed. Focuses on uptime, disaster recovery, and performance monitoring (e.g., using automated backups and failover servers).  
3. **Processing Integrity:** Ensuring data is processed accurately, completely, and on time. Critical for transaction-heavy systems like e-commerce.  
4. **Confidentiality:** Protecting sensitive information such as intellectual property or business plans through data classification and encryption.  
5. **Privacy:** Governing the collection, use, retention, disclosure, and destruction of personal data in alignment with policies and laws like GDPR or CCPA.

### **SOC 2 in the Cloud**

Cloud-based businesses must demonstrate infrastructure alignment with these principles through:

* **Access Control:** Clearly defined and documented user permissions.  
* **Change Management:** Reviewing, approving, and recording all system updates or patches.  
* **Activity Logging:** Monitoring and logging critical actions like file deletion or permission changes.  
* **Incident Response:** Maintaining clear plans to respond to and recover from breaches or outages.

### **Benefits**

* Increased customer trust and competitive advantage.  
* Improved internal documentation and security awareness.  
* Faster and more effective incident response.

## **2\. ISO/IEC 27017**

ISO 27017 is a specialized standard addressing the unique risks and responsibilities of cloud computing. it builds on ISO/IEC 27001 by adding specific guidance for cloud service providers (CSPs) and customers.

### **Key Focus Areas**

* **Shared Responsibility:** Clarifies the split between the provider (securing physical data centers and core platform) and the customer (managing users, configurations, and data security).  
* **Secure Deletion of Data:** Requires verifiable and auditable policies to ensure data is completely erased from all regions and backup systems when no longer needed.  
* **Virtual Machine (VM) Security:** Provides guidance on ensuring VMs are isolated in multi-tenant environments to prevent cross-tenant attacks.  
* **Policy and Documentation:** Requires cloud-specific security policies defining VM deployment, access grants, and data lifecycle management.

## **3\. CIS Benchmarks**

The Center for Internet Security (CIS) provides detailed, community-driven configuration guidelines for securing IT systems and cloud platforms.

### **Applicability**

* **Cloud Platforms:** AWS, Azure, and Google Cloud Platform (GCP).  
* **Operating Systems:** Linux (Ubuntu, CentOS, Debian) and Windows Server.  
* **Applications:** Web servers (Apache, NGINX) and databases (MySQL, MongoDB).  
* **Containers:** Kubernetes and Docker environments.

### **Examples of CIS Benchmarks**

* **AWS:** Ensuring S3 buckets are private by default; requiring MFA for all IAM users.  
* **Azure:** Disabling unused remote desktop ports; enforcing encryption at rest for VM disks.  
* **GCP:** Tightening IAM scopes; ensuring storage buckets are not anonymously accessible.

### **Automated Compliance Tools**

* **AWS Inspector:** Scans environments for vulnerabilities and checks against CIS Foundation benchmarks.  
* **Azure Security Center:** Monitors resources and recommends changes based on CIS standards.  
* **GCP Security Command Center:** Detects misconfigurations and security risks in projects.  
* **OpenSCAP:** Open-source tools for scanning Linux systems against CIS guidelines.

3. # **Logging and Monitoring with Cloud-Native Tools**

## **1\. The Core Principle: Visibility**

Visibility is the foundation of cloud security. You cannot protect what you cannot see. Logging and monitoring allow security teams to observe, detect, and respond to activities across cloud resources in real time or retrospectively during incident investigations.

* **Logging:** Captures a historical record of what happened, when it happened, who performed the action, and the source of the activity.  
* **Monitoring:** Actively analyzes log data to generate alerts when suspicious or unauthorized actions are detected.

### **Why it Matters**

* **Incident Detection:** Identifies unauthorized logins from foreign locations or unexpected resource deletions.  
* **Accountability:** Provides a trail of who performed specific actions and at what time.  
* **Compliance:** Essential for proving adherence to standards such as SOC2 and HIPAA.  
* **Breach Prevention:** Failing to enable these tools is a primary cause of undetected breaches in regulated industries like finance and healthcare.

## **2\. Cloud Native Tools by Provider**

### **AWS: CloudTrail**

CloudTrail is the primary service for logging API activity within an AWS account. It records actions taken by users, applications, or services—such as starting virtual machines, deleting databases, or modifying security settings.

* **Key Features:**  
  * **Automated Logging:** Tracks actions from the Management Console, SDKs, and Command Line tools.  
  * **Storage:** Logs are stored in Amazon S3 for long-term retention and review.  
  * **Integration:** Connects with Amazon CloudWatch, AWS EventBridge, or third-party SIEM tools.  
* **Use Cases:**  
  * Detecting unauthorized access and attacker behavior.  
  * Investigating resource disappearance (finding the IP address and credentials used for deletion).  
  * Demonstrating compliance regarding encryption and firewall changes.

### **Microsoft Azure: Azure Monitor**

Azure Monitor is a unified platform for collecting and analyzing telemetry from Azure services and infrastructure. It combines log data, performance metrics, and diagnostic info.

* **Key Components:**  
  * **Log Analytics:** Allows for complex data querying using Kusto Query Language (KQL).  
  * **Metrics:** Provides near real-time performance data (e.g., CPU usage, Disk I/O).  
  * **Alerts:** Triggers automated notifications or remediations (e.g., locking an account) based on specific conditions.  
  * **Security Integration:** Tightly integrated with Microsoft Defender for Cloud.  
* **Use Cases:**  
  * Monitoring login attempts to detect brute force attacks.  
  * Tracking performance to identify potential Denial of Service (DoS) attacks.  
  * Watching network traffic for signs of data exfiltration or command-and-control behavior.

### **Google Cloud (GCP): Cloud Audit Logs**

GCP provides detailed records of operations performed on resources, automatically categorized into three types:

* **Log Types:**  
  * **Admin Activity Logs:** Track configuration changes, such as IAM permission updates.  
  * **Data Access Logs:** Record read/write actions, such as accessing files in Cloud Storage.  
  * **System Event Logs:** Entries generated by Google systems, such as infrastructure maintenance.  
* **Use Cases:**  
  * Detecting misuse of service accounts or suspicious login locations.  
  * Tracking who downloaded or viewed specific files.  
  * Alerting on privilege escalation (e.g., a user granting themselves admin rights).

## **3\. Universal Best Practices**

Regardless of the provider, effective logging and monitoring should follow these principles:

1. **Enable All Relevant Logs:** Coverage should include compute, storage, network, and identity services.  
2. **Centralize Logs:** Store logs in a single, tamper-resistant location to simplify correlation and analysis.  
3. **Set Retention Policies:** Ensure logs are kept long enough to support long-term investigations and legal audits.  
4. **Configure Alerts:** Move from reactive to proactive security by notifying teams of specific behavioral patterns.  
5. **Review Regularly:** Conduct periodic reviews of anomalies early; do not wait for a major incident to look at your data.

4. # **Security Hardening in the Cloud**

## **1\. Overview of Security Hardening**

Security hardening is the process of reducing the attack surface of a cloud environment. The goal is to make the environment less vulnerable to misconfigurations, external attacks, and insider threats through:

* Tightening configurations.  
* Limiting access and applying strict policies.  
* Eliminating unnecessary services or permissions.

**Analogy:** Much like securing a house, hardening involves not just locking the front door, but also closing windows, disabling unused entrances, and installing alarms.

## **2\. Guardrails**

Guardrails are automated mechanisms that prevent risky actions before they occur. They help maintain security hygiene and compliance across rapidly growing cloud infrastructures.

### **Service Control Policies (SCPs)**

In AWS, SCPs are part of AWS Organizations and define allowed or denied actions across all member accounts.

* **Authority:** SCPs override local administrator permissions.  
* **Examples:** \* Preventing the launch of expensive GPU instances.  
  * Blocking the disabling of CloudTrail or the deletion of logs.  
  * Prohibiting the deactivation of encryption for storage services.

### **Configuration Rules**

Tools like **AWS Config**, **Azure Policy**, and **GCP Organization Policy** allow you to define the "desired state" for resources.

* **Function:** Flags configuration drifts or automatically corrects them.  
* **Examples:**  
  * Ensuring all S3 buckets have encryption enabled.  
  * Ensuring VMs do not expose Port 3389 (RDP) to the internet.  
  * Restricting IAM roles from having unapproved administrator privileges.

## **3\. Storage Security (S3 and Equivalents)**

Securing data in the cloud (S3, Azure Blob, GCP Buckets) requires robust encryption and access controls to prevent data breaches.

### **Key Principles**

* **Encryption at Rest:** Protects data stored on disks.  
* **Encryption in Transit:** Protects data as it travels across networks.  
* **Access Control:** Ensures only authorized users or applications can retrieve or modify data.

### **Best Practices**

1. **Default Encryption:** Automatically encrypt all new objects (AES-256 or AWS KMS).  
2. **IAM and Bucket Policies:** Limit access based on roles/groups (e.g., restricting access to a specific VPC).  
3. **Access Logging:** Track who accessed which files and from where. Identify unusual activity like downloads from foreign IPs.  
4. **Alerting:** Use CloudWatch (or equivalent) to alert on public access grants or unusually large data downloads.

## **4\. Network and Application Protection**

### **Web Application Firewall (WAF)**

A WAF monitors and filters HTTP/HTTPS traffic to protect web applications.

* **Common Targets:** Blocks SQL Injection, Cross-Site Scripting (XSS), and DDoS attacks.  
* **Integration:** Works with AWS CloudFront/API Gateway, Azure Front Door, and GCP Cloud Armor.  
* **Use Case:** Blocking suspicious payloads containing SQL keywords like `UNION SELECT` or geo-blocking requests from non-operating countries.

### **Security Groups (Virtual Firewalls)**

Security groups are stateful firewall rules attached to compute instances (EC2, Azure VMs).

* **Best Practices:**  
  * **Deny by Default:** Start with no access and open only necessary ports.  
  * **Restrict SSH (Port 22):** Only allow connections from trusted office IPs or VPNs.  
  * **Tiered Access:** Keep database servers in private subnets, making them inaccessible from the internet.

## **5\. Compliance and Monitoring**

Cloud security is a dynamic, layered approach requiring continuous vigilance.

* **Compliance Frameworks:** Aligning with standards like SOC 2, ISO 27017, PCI-DSS, and CIS Benchmarks.  
* **Logging Tools:** Using **AWS CloudTrail**, **Azure Monitor**, and **GCP Cloud Audit Logs** to detect anomalies and respond to incidents in real-time.

5. # **Lab: AWS Cloudtrail**

## **Introduction**

In a cloud environment, visibility is key to maintaining security and accountability. AWS CloudTrail automatically records and stores logs of all API calls and activities across your AWS accounts. This lab covers how to set up CloudTrail to track actions, monitor for suspicious behavior, and investigate incidents.

## **Lab Objectives**

* Set up a CloudTrail trail across all AWS regions.  
* Generate and review audit logs of cloud activities (login attempts, API calls, and configuration changes).  
* Utilize AWS CloudTrail for native logging and tracking.

## **Step 1: Accessing the AWS Management Console**

1. Navigate to the [AWS Management Console](https://aws.amazon.com/console/).  
2. Click **Sign In**.  
3. For this lab, sign in using the **Root user** email address and complete the login process.  
4. Enter your **Multi-Factor Authentication (MFA)** code when prompted.

## **Step 2: Creating a CloudTrail Trail**

1. In the AWS Console search bar, search for **CloudTrail**.  
2. From the CloudTrail dashboard, navigate to **Trails** in the left sidebar.  
3. Click **Create trail**.  
4. **General Details:**  
   * **Trail name:** `my-demo-trail-1`  
   * **Storage location:** Select **Create new S3 bucket**.  
   * **S3 bucket name:** Use the default name provided by AWS.  
   * **Log file integrity validation:** Select **Enabled** (used to determine if log files were modified or deleted after delivery).  
   * **SSE-KMS encryption:** Unselect **Enabled** for the purpose of this demonstration.  
   * **CloudWatch Logs:** Leave unselected (this lab focuses specifically on CloudTrail).  
5. Click **Next**.  
6. **Choose Log Events:**  
   * **Event type:** Select **Management events**.  
   * **API activity:** Ensure both **Read** and **Write** are selected to capture all management operations (logins, instance creations, etc.).  
7. Click **Next**, review the configuration, and click **Create trail**.

## **Step 3: Simulating Activity (Launching an EC2 Instance)**

To generate logs for analysis, simulate activity by launching a virtual server.

1. Open a new tab and navigate to the **EC2 Dashboard**.  
2. Click **Launch instance**.  
3. **Configuration:**  
   * **Name:** `my-demo-server-1`  
   * **Application and OS Image:** Amazon Linux (Free Tier eligible).  
   * **Instance type:** `t2.micro`.  
   * **Key pair:** Select **Proceed without a key pair** for this demo.  
   * **Network settings:** Select an **Existing security group** (e.g., "default").  
4. Click **Launch instance**.  
5. Once the instance state shows as **Running**, navigate back to the EC2 Dashboard.

## **Step 4: Stopping the Instance**

1. From the EC2 Instances list, select `my-demo-server-1`.  
2. Select **Instance state** \> **Stop instance**.  
3. Confirm the action and wait for the instance state to change to **Stopped**.

## **Step 5: Analyzing Logs in CloudTrail**

1. Return to the **CloudTrail** console and navigate to **Event history**.  
2. Review the recent logs. You should see events such as `CreateTrail`, `RunInstances`, and `StopInstances`.  
3. **Filtering Logs:**  
   * Click the **Lookup attributes** dropdown and select **Event name**.  
   * Search for `RunInstances` to see the log of the server creation.  
   * Search for `StopInstances` to see the log of the server being stopped.  
4. **Log Details:** Click on an event to view:  
   * **Who:** The username (e.g., `root`).  
   * **What:** The action performed.  
   * **When:** The exact timestamp of the activity.  
   * **Source:** The IP address and resource type (e.g., `AWS::EC2::Instance`).

## **Conclusion**

AWS CloudTrail is a critical tool for monitoring, auditing, and investigating activity within a cloud environment. By maintaining visibility over AWS accounts, security teams can quickly identify who performed an action, correlate events with login attempts, and take corrective actions like revoking credentials or blocking access.

**Key takeaway:** What you cannot see, you cannot secure. CloudTrail turns visibility into actionable security intelligence.