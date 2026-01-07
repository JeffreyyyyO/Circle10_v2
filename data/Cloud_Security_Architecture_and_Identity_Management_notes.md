# **CYS 536—WEEK 4**

## **CLOUD SECURITY ARCHITECTURE AND IDENTITY MANAGEMENT**

1. # **The Shared Responsibility Model in Cloud Security**

## **Introduction**

As organizations shift to the cloud, understanding how to secure the environment is essential. This guide covers the Shared Responsibility Model (SRM) across AWS, Azure, and GCP, core security principles, and identity management.

## **1\. The Shared Responsibility Model (SRM)**

The Shared Responsibility Model is foundational to cloud security. It defines the division of security obligations between the Cloud Service Provider (CSP)—such as AWS, Microsoft Azure, or Google Cloud Platform (GCP)—and the customer (the organization or development team).

Unlike traditional on-premise setups where the organization is responsible for every layer (from physical hardware to software), cloud responsibilities are shared depending on the service model used.

## **2\. Cloud Service Models and Responsibility**

The division of responsibility depends heavily on whether you are using **IaaS**, **PaaS**, or **SaaS**.

### **Infrastructure as a Service (IaaS)**

* **Definition:** Provides foundational building blocks like virtual machines, storage, and networks.  
* **Analogy:** Like renting an **unfurnished apartment**. The landlord provides the building and utilities, but you must bring your own furniture, maintain your space, and lock your doors.  
* **Examples:** AWS EC2, Azure VMs, Google Compute Engine.  
* **Provider Responsibilities:** Physical security of data centers, network infrastructure, and the virtualization layer (hypervisor).  
* **Customer Responsibilities:** Operating system maintenance, patching, firewall configuration, application security, and Identity and Access Management (IAM).

### **Platform as a Service (PaaS)**

* **Definition:** Provides a managed environment for application deployment, abstracting away the underlying infrastructure.  
* **Analogy:** Like moving into a **serviced apartment**. Most utilities and maintenance are handled by the landlord, while you focus on living your life (running your application).  
* **Examples:** AWS Elastic Beanstalk, Azure App Service, Google App Engine.  
* **Provider Responsibilities:** Everything in IaaS plus the host operating system, runtime environment (web servers, language support), and automatic patching/scaling.  
* **Customer Responsibilities:** Application code/logic, data input/output, and application-level authentication.

### **Software as a Service (SaaS)**

* **Definition:** Delivers fully functional software over the internet. The provider handles almost everything.  
* **Analogy:** Like staying in a **hotel**. Everything is ready for you; you are responsible for what you do inside your room, but not for maintaining the facility.  
* **Examples:** Microsoft 365, Google Workspace, Salesforce.  
* **Provider Responsibilities:** Infrastructure, application runtime, platform security, and availability.  
* **Customer Responsibilities:** Configuring user access/roles, managing data within the software, and ensuring proper data governance.

## **3\. Provider-Specific Models**

### **Amazon Web Services (AWS)**

* **AWS Managed:** Physical data center security (locked cages, power), hardware, and the virtualization layer.  
* **Customer Managed:** Data confidentiality/integrity, application updates, IAM policies, and security groups (firewalls).  
* *Example:* On an EC2 instance, AWS secures the server hardware, but you must patch the OS and restrict user access.

### **Microsoft Azure**

* **Azure Managed:** Physical data centers, host operating systems, and foundational networking.  
* **Customer Managed:** End-user access, data classification, and application-level controls (e.g., token expiry).  
* *Example:* Using Azure App Services, Microsoft handles system uptime; you handle API keys and admin dashboard access.

### **Google Cloud Platform (GCP)**

* **GCP Managed:** Global infrastructure, physical security, and the network backbone.  
* **Customer Managed:** VM configuration, Role-Based Access Control (RBAC), and data lifecycle management.  
* *Example:* On Cloud SQL, Google patches the database engine, but you are responsible for encryption at rest and least-privilege access.

## **4\. Summary of Responsibilities**

| Component | IaaS | PaaS | SaaS |
| ----- | ----- | ----- | ----- |
| **Physical Infrastructure** | Provider | Provider | Provider |
| **Network & Virtualization** | Provider | Provider | Provider |
| **Operating System & Runtime** | **Customer** | Provider | Provider |
| **Application Code** | **Customer** | **Customer** | Provider |
| **Data & User Access** | **Customer** | **Customer** | **Customer** |

**Note:** Regardless of the platform or model, the **customer is always responsible for their data and user access.**

## **5\. Certification & Professional Practice**

The Shared Responsibility Model is a core concept tested in major security certifications:

* **AWS:** Certified Security – Specialty  
* **Azure:** SC-900 (Security, Compliance, and Identity Fundamentals)  
* **GCP:** Professional Cloud Security Engineer

Understanding this model is vital not just for exams, but to prevent real-world security breaches caused by neglecting customer-side responsibilities.

2. # **Core Security Principles in Cloud Architecture**

## **Fundamental Principles of Cloud Security**

Security in cloud environments must be intentionally designed into every layer. There are three fundamental principles that guide the construction of secure cloud systems: Network Segmentation, Identity and Access Management (IAM), and Encryption.

## **1\. Network Segmentation**

Network segmentation is the practice of dividing a cloud network into smaller, isolated segments or zones. This limits inter-system communication and reduces the risk of lateral movement if a system is compromised.

### **Key Concepts**

* **Virtual Private Cloud (VPC):** A private data center in the cloud. It provides control over IP address ranges, subnets, route tables, and gateways. Used in AWS, Azure, and GCP.  
* **Subnets:** Subdivisions within a VPC.  
  * **Public Subnets:** For resources requiring internet access (e.g., web servers).  
  * **Private Subnets:** For internal systems (e.g., databases).  
* **Network Security Groups (NSGs):** Act as firewalls to define which traffic can enter or leave a subnet, virtual machine (VM), or specific resource.

### **Practical Application**

In an e-commerce application, a public-facing web server is placed in a public subnet, while the database server is placed in a private subnet with no direct internet access. Security rules are configured so that only the web server can communicate with the database over a specific port (e.g., port 3306 for MySQL).

## **2\. Identity and Access Management (IAM)**

IAM is a framework used across providers (AWS IAM, Microsoft Entra ID/Azure AD, and GCP IAM) to define who can access cloud resources and what actions they can perform.

### **Key Concepts**

* **Users and Groups:** Individuals or teams requiring resource access.  
* **Roles and Permissions (Policies):** Defined sets of allowed actions, such as read-only access to storage buckets or full access to deploy applications.  
* **Principle of Least Privilege:** Users are granted only the minimum access necessary to perform their jobs, reducing the risk of accidental or malicious damage.

### **Practical Application**

* **Junior Developer:** Access restricted to development and staging compute resources.  
* **DevOps Engineer:** Permissions to deploy to production, but no access to billing.  
* **Finance Officer:** Read-only access to billing dashboards, but no access to cloud infrastructure.

## **3\. Encryption**

Encryption ensures that data is unreadable to unauthorized users, whether it is stored or moving across networks.

### **Encryption at Rest**

This applies to data stored on disks and in storage services.

* **Storage Services:** AWS S3, Azure Blob Storage, GCP Cloud Storage.  
* **Databases:** Amazon RDS, Azure SQL, Google BigQuery.  
* **Management Models:**  
  * **Server-Side Encryption (SSE):** The cloud provider manages the encryption process.  
  * **Customer Managed Keys (CMKs):** Users control the encryption keys using services like AWS KMS, Azure Key Vault, or GCP KMS.

### **Encryption in Transit**

This applies to data moving between users and applications or between internal services.

* **Protocols:** HTTPS and TLS are used to encrypt web traffic.  
* **Secure Tunnels:** VPNs or private links encrypt traffic between on-premises and cloud environments.

### **Practical Application**

* **At Rest:** Data stored in an AWS S3 bucket with SSE enabled is encrypted by AWS before being written to disk.  
* **In Transit:** An application enforcing HTTPS ensures that anyone intercepting the traffic (e.g., via a packet sniffer) sees only encrypted gibberish.

## **Summary of Principles**

| Principle | Security Goal | Tools & Examples |
| ----- | ----- | ----- |
| **Network Segmentation** | Prevents lateral movement | VPCs, Subnets, NSGs, Firewalls |
| **IAM** | Controls user access | Roles, Policies, Groups, Entra ID |
| **Encryption** | Protects confidentiality | KMS, TLS/HTTPS, SSE, Key Vault |

3. # **Cloud-Native Security Tools**

## **Introduction**

As organizations move workloads to the cloud, traditional security tools are often insufficient. Major cloud providers—AWS, Azure, and GCP—each offer built-in, cloud-native security tools designed to monitor access and protect cloud environments continuously. These tools help centralize security management, provide alerts, and enforce best practices.

## **1\. AWS Security Hub**

AWS Security Hub is a central dashboard that collects and displays findings from various AWS security services, providing a comprehensive view of your security posture.

* **Key Functions**: Aggregates alerts from services like:  
  * **Amazon GuardDuty**: Threat detection.  
  * **Amazon Inspector**: Vulnerability scanning.  
  * **Amazon Macie**: Sensitive data discovery (e.g., PII in S3).  
* **Compliance**: Provides a consolidated view of compliance with industry standards like CIS benchmarks or PCI-DSS.  
* **Actionable Insights**: Offers specific recommendations to remediate issues.  
* **Example**: It can alert you if an S3 bucket is publicly accessible or if the Root account does not have Multi-Factor Authentication (MFA) enabled.

## **2\. Microsoft Defender for Cloud (formerly Azure Security Center)**

Microsoft Defender for Cloud is a unified security management system that monitors workloads across Azure, hybrid, and multi-cloud environments.

* **Key Functions**:  
  * **Continuous Scanning**: Monitors infrastructure for misconfigurations, such as open ports or outdated libraries.  
  * **Secure Score**: A metric that quantifies the security of your environment and provides a roadmap for improvement.  
  * **Threat Protection**: Includes compliance monitoring and proactive recommendations.  
* **Example**: In an Azure Kubernetes Service (AKS) environment, it can identify if a container image contains a known vulnerability (e.g., an unpatched version of OpenSSL) and recommend applying a security patch.

## **3\. GCP Security Command Center (SCC)**

Security Command Center is Google Cloud’s centralized platform for monitoring and managing security risks.

* **Key Functions**:  
  * **Asset Inventory**: Provides a real-time inventory of all cloud resources.  
  * **Vulnerability Detection**: Identifies misconfigured firewalls and exposed services.  
  * **Integration**: Connects with other GCP services like:  
    * **Cloud Armor**: Web Application Firewall (WAF).  
    * **Chronicle**: Security analytics and threat detection.  
    * **Cloud DLP**: Data Loss Prevention.  
* **Example**: If a Compute Engine VM is created with port 22 (SSH) open to the internet, SCC flags this as a high-risk firewall rule. It also identifies improperly configured IAM policies or unencrypted data buckets.

## **Comparison Summary**

| Tool | Cloud Provider | Key Features | Real-World Example |
| ----- | ----- | ----- | ----- |
| **Security Hub** | AWS | Aggregates alerts (GuardDuty, Macie, etc.) | Alerts when an S3 bucket is made public. |
| **Defender for Cloud** | Azure | Secure Score, threat protection, compliance | Detects vulnerable container images. |
| **Security Command Center** | GCP | Asset inventory, threat detection, IAM insights | Flags VMs with exposed firewall rules. |

4. # **Identity Federation and Single Sign-On (SSO)**

## **Introduction**

In the modern cloud environment, managing user access has become increasingly complex. Organizations must manage access for employees, third-party vendors, and partners across multiple platforms (AWS, Azure, GCP) and SaaS applications (Salesforce, Slack, Office 365). Identity Federation and Single Sign-On (SSO) are the primary tools used to scale access securely, reduce password fatigue, and centralize authentication.

## **Identity Federation**

Identity Federation is a method that allows users from an external **Identity Provider (IdP)**—such as Okta, Google Workspace, or Active Directory—to access services in a cloud environment without creating separate user accounts for each platform.

**The Passport Analogy:** Federation works like an international passport agreement. You use your home country’s passport (your main identity) to enter other countries (cloud systems) without needing to apply for a new local ID every time.

### **How Identity Federation Works Step-by-Step**

1. **Establishing Trust (Setup Phase):** Before access is granted, the organization creates a trust relationship between the Cloud Provider (AWS, Azure, GCP) and the IdP (Okta, Google, etc.). This involves:  
   * Configuring the cloud provider to accept security tokens from the IdP.  
   * Defining roles and permissions in the cloud that map to specific users or groups in the IdP.  
   * Establishing secure communication protocols, typically **SAML** (Security Assertion Markup Language) or **OIDC** (OpenID Connect).  
2. **Authentication at the IdP:** The user does not log into the cloud provider first. Instead, they visit a company-specific login page managed by the IdP. The user enters their credentials (username, password) and completes Multi-Factor Authentication (MFA).  
3. **The IdP Sends a Secure Token:** After successful authentication, the IdP generates a secure, digitally signed token containing:  
   * The user's identity (email, ID, group membership).  
   * A digital signature to prevent tampering.  
   * Session duration and specific entitlements.  
4. **Verification and Access:** The cloud provider receives and verifies the token's digital signature. It then maps the user to predefined roles/policies and grants temporary security credentials, allowing the user to access the console or services without a local password.

## **Understanding Security Tokens: SAML vs. OIDC**

| Feature | SAML (Security Assertion Markup Language) | OIDC (OpenID Connect) |
| ----- | ----- | ----- |
| **Analogy** | A passport with physical XML stamps. | A digital driver’s license in JSON format. |
| **Format** | XML (Older, enterprise standard). | JSON / JWT (Modern, lightweight, efficient). |
| **Common Use** | Enterprise SSO (e.g., Okta to AWS). | Modern web apps and APIs (e.g., Google Sign-In). |

## **Single Sign-On (SSO)**

SSO allows users to authenticate once per session and gain access to multiple independent systems without being prompted for credentials again for each one.

### **Key Benefits**

* **Improved User Experience:** One login for all tools.  
* **Reduced Password Fatigue:** Fewer passwords for users to remember.  
* **Minimizes Phishing Risks:** Fewer login prompts reduce the opportunities for credential theft.  
* **Centralized Security:** Consistent enforcement of MFA and conditional access policies.

### **Workflow**

1. A central IdP (Azure AD, Okta) handles the session.  
2. When the user logs into the first system, a session token is generated.  
3. When accessing subsequent applications, that token is passed along and trusted automatically.

## **Real-World Scenario: A Federated Workday**

In a company using Okta as its IdP, the workflow looks like this:

1. **Morning Login:** The employee logs into the Okta portal once with MFA.  
2. **Seamless Access:**  
   * Clicking **Salesforce** instantly logs the user in via a SAML token.  
   * Opening **Slack** or **Google Workspace** requires no further login.  
   * Accessing the **AWS Console** uses a federated token to assign the correct IAM role.  
3. **Session Expiry:** For security, the Okta session may expire after several hours, requiring one re-authentication to regain access to all tools simultaneously.

## **Cybersecurity Implications**

Without SSO and Federation, users would manage 4–5 different passwords daily, leading to weak password reuse and higher support costs. Centralizing identity management allows an organization to:

* Reduce the attack surface.  
* Enforce strong MFA across all services.  
* **Instantly revoke access** to all integrated services from a single location if an employee leaves the organization.

In a cloud-first world, identity management is the foundational backbone of security architecture, moving beyond traditional perimeter defense to a model where identity is the core security boundary.

5. # **Designing A Secure Cloud Architecture Using Lucid Charts And Control Mapping**

## **Designing a Secure Cloud Architecture**

This lesson covers the visualization and security of a cloud environment using Lucidchart and control mapping. The focus is on integrating infrastructure design with Identity and Access Management (IAM) and security controls.

## **Learning Objectives**

The session is structured around three primary tasks:

1. **Cloud Architecture Design:** Creating a diagram in Lucidchart including VPCs, subnets, EC2, RDS, and S3.  
2. **IAM Policy Review:** Assigning specific policies to roles such as Administrator, Developer, and Auditor.  
3. **Security Control Mapping:** Aligning architectural components with industry best practices (NIST 800-53, SOC 2, and ISO 27017).

## **1\. Scenario: E-Commerce Web Application**

The demo focuses on a simple three-tier e-commerce application hosted on **AWS** using the following components:

* **Public Tier:** Public-facing web server (HTTP/HTTPS).  
* **Application Tier:** Internal application server.  
* **Database Tier:** Secure, encrypted backend database.

### **Architectural Requirements**

* **Network:** 1 VPC in a single Region with 4 subnets (1 Public, 3 Private).  
* **Traffic Management:** Route 53 (DNS) and an Application Load Balancer (ALB).  
* **Security:** AWS Network Firewall.

## **2\. Infrastructure Components**

### **Network & Entry Points**

* **Route 53:** Serves as the DNS service.  
* **AWS Network Firewall:** Positioned between the VPC entry and the public subnets to protect the entire network.  
* **Application Load Balancer (ALB):** Located within the public subnet to distribute traffic to the web servers.  
* **Amazon EC2:** Serves as the web server endpoint.

### **Encryption Zone (Private Subnets)**

Sensitive data is isolated within an encryption-focused private subnet:

* **Amazon S3:** Used for object storage.  
* **Amazon RDS:** The relational database backend.  
* **AWS KMS (Key Management Service):** Provides encryption for data at rest within both S3 and RDS.  
* **AWS Backup:** Configured to perform daily backups of RDS and S3 for disaster recovery.

## **3\. Identity and Access Management (IAM)**

Access is governed by the Principle of Least Privilege across three defined roles:

| Role | Access Level | Description |
| ----- | ----- | ----- |
| **Administrator** | Full Access | Manage all VPC resources, subnets, and security configurations. |
| **Developer** | Read-Only / Deploy | View VPC/Subnets and deploy application services; cannot modify network infrastructure. |
| **Auditor** | Limited Audit | Access to logs and resource configurations required for compliance verification. |

### **Policy Examples (JSON Logic)**

* **Administrator Policy:** Uses `Action: "*"` on VPC resources to allow creation, modification, and deletion of subnets and instances.  
* **Developer Policy:** Restricts actions to `ec2:Describe*` and `vpc:Describe*` to allow visibility for deployment without permission to change the underlying architecture.

## **4\. Monitoring and Logging**

To ensure visibility and traceability, the following tools are integrated:

* **Amazon CloudWatch:** Collects operational metrics, sets alarms, and monitors system health for EC2, RDS, and Lambda.  
* **AWS CloudTrail:** Records all API activity within the AWS account to track who did what and when.  
* **Integration:** Integrating CloudWatch with CloudTrail allows for automated alerting based on specific API activities.

## **5\. Compliance Mapping**

Security controls are mapped to recognized frameworks to ensure the architecture meets government or industry standards.

| Control Name | Framework | Implementation Detail |
| ----- | ----- | ----- |
| **Access Control (AC-2)** | NIST 800-53 | Defined IAM roles for Admin, Developer, and Auditor. |
| **Data Encryption (C.12)** | ISO 27017 | AWS KMS used to encrypt data at rest in S3 and RDS. |
| **Audit Logging (AU-2)** | SOC 2 | CloudTrail and CloudWatch implemented for API and system logging. |
| **System Monitoring (SI-4)** | NIST 800-53 | CloudWatch dashboards and alerts for system health. |
| **Backup & Recovery (CP-9)** | ISO 27017 | Automated daily backups via AWS Backup. |

## **Conclusion**

A resilient cloud environment requires that architectural design, access management, and security governance work in tandem. By visually mapping controls onto an infrastructure diagram, organizations can enforce clear access boundaries and ensure continuous compliance.
