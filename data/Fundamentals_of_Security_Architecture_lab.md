# **CYS 530—WEEK 1—LAB**

## **FUNDAMENTALS OF SECURITY ARCHITECTURE**

1. # **Identifying Key Components and Security Challenges in Enterprise Architecture**

## **Overview**

This laboratory module focuses on applying security architecture principles within an enterprise environment. By analyzing a typical e-commerce platform, learners will identify critical system components, understand their functions, and recognize how attackers exploit specific weaknesses.

## **Enterprise Architecture: E-Commerce Case Study**

An e-commerce architecture is comprised of five key integrated components. Understanding the flow of data between these components is essential for effective threat modeling.

### **Core Components**

1. **Web Front-End (Customer Portal):** The user interface, including websites and mobile applications, where users browse products, manage carts, and initiate checkouts. It handles visual elements, forms, session management, and initial data communication.  
2. **Application Backend:** The "brain" of the application. It processes business logic, validates user requests, manages inventory, and coordinates communication between the front-end, database, and payment services.  
3. **Database Server:** The system's memory. It stores critical information including customer profiles, product details, order histories, shipping addresses, and payment records.  
4. **Payment Processing Service:** Orchestrates the secure handling of transactions. It collects payment information, prepares it for validation, and manages status updates (e.g., success, pending, failure).  
5. **External Payment Gateway:** A third-party service (e.g., Stripe, PayPal, Paystack) that performs the final verification and transaction processing. While external, it is a critical part of the attack surface.

## **Architectural Data Flow**

Effective security requires understanding how these components communicate:

* The **Web Front-End** communicates with the **Application Backend** and the **Payment Processing Service**.  
* The **Application Backend** interacts with the **Database Server** and the **Payment Processing Service**.  
* The **Payment Processing Service** maintains a bidirectional link with the **External Payment Gateway**.

## **Visualizing the Architecture: Step-by-Step Lab**

To effectively threat model, you must first visualize the system's data flow. This section walks through using [draw.io](https://app.diagrams.net/) to create your architectural diagram.

### **1\. Environment Setup**

* **Access the Platform:** Open your web browser and navigate to `draw.io` or `diagrams.net`.  
* **Initialize:** Select your preferred workspace or profile to begin.

### **2\. Mapping Components**

* **Selecting Shapes:** Use the `General` tab to select standard shapes like rectangles.  
* **Labeling:** Drag a shape onto the canvas and double-click it to add component labels (e.g., "Web Front-End / Customer Portal").  
* **Extending:** Add additional rectangles for the Application Backend, Database Server, Payment Processing Service, and External Payment Gateway.

### **3\. Defining Data Flow**

* **Connectors:** Hover over an existing shape to see arrow connectors. Drag these arrows to link components.  
* **Directionality:** Configure connectors as either one-way or two-way arrows depending on the communication flow (e.g., a two-way connection for the Payment Processing Service and External Payment Gateway).

### **4\. Styling and Customization**

* **Formatting:** Use the formatting panel to adjust colors, gradients, and font styles (e.g., using Georgia font) for better readability.  
* **Line Styles:** Adjust connector lines—use dashed lines or specific arrowheads to differentiate between internal and external communication or to denote specific data states.  
* **Consistency:** Maintain a professional look by ensuring consistent sizing and alignment across all components.

## **Security Challenges and Mitigations**

Building upon the architectural visualization, it is critical to address the inherent security challenges within each component.

### **A. Web Front-End (Customer Portal)**

* **Key Challenges:**  
  * **Cross-Site Scripting (XSS):** Injection of malicious scripts that steal cookies or session tokens.  
  * **Cross-Site Request Forgery (CSRF):** Tricking users into performing unauthorized actions.  
  * **Insecure Direct Object References (IDOR):** Unauthorized access to data by manipulating URL parameters (e.g., invoice IDs).  
  * **Insufficient Input Validation:** Vulnerability to various injection attacks.  
* **Mitigation Strategies:**  
  * Implement robust input sanitization and output encoding.  
  * Utilize CSRF tokens.  
  * Enforce strict access control policies.  
  * Conduct regular front-end code audits.

### **B. Application Backend**

* **Key Challenges:**  
  * **Insecure APIs:** Improper authentication or over-exposure of information.  
  * **Business Logic Flaws:** Errors in how the system processes data/requests.  
  * **Insufficient Access Controls:** Failure to enforce permissions.  
  * **Injection Vulnerabilities:** (SQL, Command, NoSQL) Execution of untrusted input.  
* **Mitigation Strategies:**  
  * Integrate secure, vetted API gateways.  
  * Adhere to the **Principle of Least Privilege**.  
  * Perform periodic logic and authorization reviews.  
  * Validate all inputs on the server side.

### **C. Database Server**

* **Key Challenges:**  
  * **SQL Injection:** Malicious queries to read or modify data.  
  * **Excessive Privileges:** Providing unnecessary access to services/users.  
  * **Unencrypted Sensitive Data:** Storing data (passwords, card details) in plain text.  
  * **Weak Authentication:** Lack of MFA and weak password policies.  
* **Mitigation Strategies:**  
  * Use prepared statements or an ORM to prevent SQL injection.  
  * Encrypt sensitive data at rest and in transit.  
  * Enforce Role-Based Access Control (RBAC).  
  * Mandate strong passwords and Multi-Factor Authentication (MFA).

### **D. Payment Processing Service**

* **Key Challenges:**  
  * **Unencrypted Payment Data:** Interception risks.  
  * **Weak API Authentication:** Unauthorized access to endpoints.  
  * **Lack of Transaction Logging:** Difficulty in tracing fraud.  
* **Mitigation Strategies:**  
  * Mandate TLS/SSL for all payment traffic.  
  * Implement tokenization to avoid storing real card numbers.  
  * Maintain secure transaction logs and monitor for anomalies.

### **E. External Payment Gateway**

2. **Key Challenges:**  
   1. **Insecure API Endpoints:** Lack of validation or filtering.  
   2. **Weak Certificate Validation:** Accepting expired/invalid SSL certificates.  
   3. **Vendor Security Posture:** Inherited risks from third-party partners.  
3. **Mitigation Strategies:**  
   1. Thoroughly vet all third-party providers.  
   2. Validate SSL/TLS certificates and enforce HTTPS.  
   3. Establish clear Service Level Agreements (SLAs) and incident response protocols.

4. # **Applying Security Principles to an Unsecured Network Architecture**

## **Part 1: Introduction and Lab Objectives**

### **Overview**

This lab exercise focuses on the practical application of cybersecurity principles to network design. The primary goal is to contrast an inherently insecure network architecture with a robust, secured counterpart. By analyzing a poorly designed model, participants learn to identify critical vulnerabilities and implement effective defensive strategies.

### **Lab Objectives**

* **Analyze Vulnerabilities:** Examine a sample network architecture intentionally designed to be insecure.  
* **Understand Security Principles:** Apply core security concepts to transform an unsecured layout into a hardened, production-ready environment.  
* **Practical Application:** Work through a real-world scenario to understand how architectural choices directly impact an organization's security posture.

## **Part 2: Structural Analysis of an Insecure Network Architecture**

### **Architecture Overview**

To understand defense, we must first understand common failure points. The initial lab architecture includes:

* **Direct Internet Access:** An internet connection feeding directly into a router.  
* **Lack of Perimeter Defense:** The router lacks a firewall, allowing all incoming traffic to reach internal resources without inspection.  
* **Flat Network Design:** No segmentation exists between the web, application, and database tiers.  
* **Public Exposure:** All critical servers (Web, App, and Database) are assigned public IP addresses, making them directly reachable from the open internet.  
* **Unprotected Management:** Administrative consoles on the application servers are exposed to the public internet and lack any authentication mechanisms.

### **Security Vulnerabilities**

The following critical flaws demonstrate why this architecture is a "blueprint for disaster":

1. **Lack of Network Segmentation:** The network is entirely flat. Once an attacker compromises a single device, they can scan and traverse the entire network, compromising all tiers.  
2. **No Firewall Protection:** The absence of border and host-based firewalls means there is no mechanism to filter traffic or block unauthorized connections.  
3. **Public IP Exposure:** Exposing application and database servers to the internet significantly expands the attack surface.  
4. **Unauthenticated Admin Interfaces:** The absence of username/password requirements or Multi-Factor Authentication (MFA) on admin consoles allows attackers to take control of systems with zero effort.  
5. **Lack of Encryption:** Traffic between servers (data-in-transit) is not encrypted, making it vulnerable to interception and Man-in-the-Middle (MitM) attacks.  
6. **Excessive Privileges:** Services are running with full administrative privileges. A compromise in any single service grants the attacker complete control over the host operating system.

## **Part 3: Transitioning to a Secured Network Architecture**

### **Architectural Overview**

The redesigned architecture shifts from a flat, exposed structure to a layered, defended environment. The core focus is on segmenting services and enforcing access control.

### **Layered Components and Functions**

1. **Border Firewall:** Situated between the internet and internal resources, it acts as the first line of defense, inspecting inbound/outbound traffic and enforcing rules based on IP ranges and protocols.  
2. **Demilitarized Zone (DMZ):** A hardened environment hosting public-facing services (e.g., Web Servers/APIs). It isolates external-facing components from core internal assets.  
3. **IDS/IPS Monitoring:** Continuous monitoring of the DMZ traffic.  
   * **IDS (Intrusion Detection System):** Identifies suspicious patterns and alerts administrators.  
   * **IPS (Intrusion Prevention System):** Actively blocks malicious traffic in real-time.  
4. **Internal Firewall:** Acts as a second protective boundary, governing East-West traffic (communication between internal tiers) and ensuring that only authorized traffic moves between zones.  
5. **Tiered Subnets (Web/App/Database):**  
   * **Web Tier:** Processes requests from the DMZ.  
   * **Application Tier:** Runs core business logic; restricted communication with the Web tier only.  
   * **Database Tier:** Stores sensitive data; strictly isolated from the internet and directly reachable only by the Application tier.  
6. **Management Network & Jump Server:** Administrative tasks are performed through a dedicated jump server requiring Multi-Factor Authentication (MFA). This removes the need for exposed admin consoles.

### **Core Security Principles Applied**

* **Defense in Depth:** Implementing multiple layers of security so that if one fails, others are in place to stop the threat.  
* **No Single Point of Failure:** Designing systems so that a compromise in one component (e.g., a web server) does not automatically result in the compromise of the entire network.  
* **Least Privilege:** Ensuring users and services operate only with the minimum access required for their function (e.g., avoiding root privileges for applications).  
* **Network Segmentation:** Using subnets and firewalls to partition the network, which contains threats and prevents lateral movement by attackers.

5. # **Threat Modeling Using STRIDE**

## **Part 1: Architecture Setup**

### **Introduction to the STRIDE Framework**

STRIDE is a threat modeling framework developed by Microsoft to help identify security threats in software systems by breaking them down into six major categories:

* **S \- Spoofing:** Pretending to be someone or something else.  
* **T \- Tampering:** Modifying data or code in an unauthorized way.  
* **R \- Repudiation:** Claiming that you didn't perform an action.  
* **I \- Information Disclosure:** Exposing sensitive information to unauthorized parties.  
* **D \- Denial of Service:** Making a system or resource unavailable.  
* **E \- Elevation of Privilege:** Gaining unauthorized higher-level access.

### **Lab Objectives**

1. **Analyze** the architecture of a simplified e-commerce system.  
2. **Identify** specific threats at various system components and data flows.  
3. **Propose** mitigation strategies early in the design or review stage.

## **System Architecture**

The lab uses a simplified e-commerce architecture consisting of:

1. **Users:** External clients initiating requests via the internet.  
2. **Load Balancer:** Distributes traffic across web servers.  
3. **Web Servers (x2):** Host the frontend application and handle HTTP requests.  
4. **Application Server:** Handles business logic and backend processing.  
5. **Database:** Stores customer data, transaction records, and product information.  
6. **Payment API (Third-Party):** External service integration for processing financial transactions.

## **Part 2: Threat Mapping and Mitigation**

By applying the STRIDE framework, we can map potential threats to specific architectural components and define proactive mitigation strategies.

### **STRIDE Threat Mapping Table**

| Component | Threat Category | Example Threat | Mitigation Strategy |
| ----- | ----- | ----- | ----- |
| **Load Balancer** | Denial of Service | DDoS attack overwhelming entry points. | Cloud-based DDoS protection, rate limiting, geoblocking. |
| **Load Balancer** | Spoofing | Attacker pretending to be a trusted client. | Mutual TLS (mTLS), IP allow-listing. |
| **Web Servers** | Tampering | Modifying frontend JavaScript files. | File Integrity Monitoring (FIM), CDNs with signed content. |
| **Web Servers** | Information Disclosure | Verbose error messages exposing stack details. | Custom error pages, disabling stack traces, sanitizing logs. |
| **Web Servers** | Spoofing | Impersonating user sessions (stolen cookies). | Secure cookies, HTTP-only flags, MFA. |
| **App Server** | Elevation of Privilege | Logic flaw allowing standard users to gain admin rights. | Role-Based Access Control (RBAC), input validation. |
| **App Server** | Repudiation | Denying the initiation of specific actions/transactions. | Detailed audit logging, digital signatures, timestamping. |
| **App Server** | Denial of Service | Resource-intensive API calls slowing processing. | Rate limiting, circuit breakers, usage quotas. |
| **Database** | Information Disclosure | SQL injection to extract customer data. | Parameterized queries, encryption at rest and in transit. |
| **Database** | Tampering | Unauthorized alteration of data records. | Strict access controls, database activity monitoring. |
| **Payment API** | Spoofing | Intercepting card details via fake endpoints. | TLS with certificate pinning, domain validation. |
| **Payment API** | Repudiation | Vendor denying receipt of transaction requests. | Signed receipts, traceable transaction IDs, tamper-proof logs. |

### **Summary of STRIDE Controls**

* **Spoofing:** Mitigated via identity validation (certificates, tokens, MFA).  
* **Tampering:** Mitigated via integrity checks (WAFs, FIM, checksums).  
* **Repudiation:** Mitigated via accountability mechanisms (signed logs, timestamps).  
* **Information Disclosure:** Mitigated via confidentiality controls (encryption, access management).  
* **Denial of Service:** Mitigated via availability safeguards (rate limiting, auto-scaling).  
* **Elevation of Privilege:** Mitigated via strict authorization (least privilege, RBAC).

**Conclusion:** Threat modeling forces a shift in perspective—thinking like an attacker to design proactive, resilient defenses. This promotes a stronger security posture and fosters a culture of security awareness across the entire development lifecycle.