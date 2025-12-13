# **Month 1, Week 1:**

# **Fundamentals of Network Security**

### **Overview**

* Introduction to Network Security  
* Tools and Techniques for Network Security  
* OSI Model and Network Security  
* Why Network Security Matters  
* Detailed Study of Network-Based Security Threats

# **Introduction to Network Security**

Network security involves the practices and technologies used to protect computer networks and the data they carry from unauthorized access, misuse, and attacks. It ensures the confidentiality, integrity, and availability of a network and its resources through a combination of hardware, software, policies, and protocols. Key components include firewalls, encryption, and access control, with the ultimate goal of safeguarding sensitive information and maintaining operational continuity.

**Core principles and goals**

* **Confidentiality:** Ensuring that data is accessible only to authorized users.  
* **Integrity:** Protecting data from unauthorized modification or tampering.  
* **Availability:** Making sure that the network and its resources are accessible when needed.

## **What Is Network Security?**

Network security refers to the technologies, policies, people, and procedures that defend any communication infrastructure from cyberattacks, unauthorized access, and data loss, while upholding the principles of the CIA triad. In addition to the network itself, they also secure traffic and network-accessible assets at both the network edge and inside the perimeter.

# **Tools & Techniques For Network Security**

## **Tools & Techniques For Network Security**

Tools and techniques for network security include:

* Firewalls  
* Intrusion Prevention Systems  
* Antivirus And Sandboxing  
* Endpoint Detection And Response (EDR)  
* Email Security  
* Data Loss Prevention (DLP)  
* Distributive Denial Of Service Protection  
* Application Security  
* Cloud Access Security Broker (CASB)

### **Firewalls**

Firewalls are essential security tools that act as a barrier between a trusted network and untrusted networks, like the internet. They filter network traffic based on predefined rules, preventing unauthorized access and malicious attacks.

### **Intrusion Prevention Systems**

Intrusion prevention systems: detect and block known and suspected threats before they can impact the network core or devices at its edge. In addition to north/south and east/west deep packet inspection, including inspection of encrypted traffic, they can also provide virtual patching, which mitigates vulnerabilities at the network level.

Using an IPS, organizations can rapidly detect attack signatures and abnormal behavior. The system automatically takes action to block malicious traffic while alerting administrators for further investigation.

### **Antivirus & Sandboxing**

Antivirus and sandboxing tools are key to determining whether a file is malicious. While antivirus blocks known malware threats, sandboxing provides a safe environment to analyze suspicious files.

Let's say a user downloads a file from an email attachment. The antivirus software scans it for known attack signatures and behaviors. If it's a confirmed threat, the software quarantines or removes the file. For an unknown file, sandboxing isolates it into a protected space where it can be tested to determine if it's malicious.

Some security vendors are leveraging these capabilities in concert with AI, allowing them to perform sub-second analysis of never-before-seen threats.

### **Endpoint Detection And Response (EDR)**

**Endpoint Detection and Response (EDR)** security solutions continuously monitor all user and endpoint activities to protect them from threats and detect suspicious behavior. They also offer investigation and incident response capabilities, which eradicate the threat and isolate the affected system from impacting the rest of the network.

### **Email Security**

Email security protects employees from cyber threats and social engineering attacks, including phishing, spear-phishing, and other email-based tactics. It works by examining inbound email to detect potential risk factors. For instance, it can identify communications that contain malware, suspicious links, questionable content and imagery, and other abnormalities \- blocking the email from reaching the user's inbox.

### **Data Loss Prevention (DLP)**

DLP solutions identify sensitive information throughout cloud systems, mitigate accidental data sharing, and prevent data exfiltration. They increase visibility into data stores, helping enterprises track and remediate policy violations, thereby improving internal compliance.

### **Distributive Denial Of Service Protection**

DDoS protection defends against denial-of-service attacks, which aim to overwhelm the corporate network and disrupt operations. **FortiDDoS** and **SophosDDoS,** for example, rapidly inspects data packets and automatically blocks illegitimate traffic from flooding the network.

### **Application Security**

Application security tools allow administrators to recognize traffic generated by well-known and custom applications. Using IPS protocol decoders, it analyzes traffic to identify applications, enabling admins to quickly form policies to allow, deny, or restrict access to specific apps or entire categories of them. This optimizes bandwidth utilization while also mitigating malware, unauthorized file transfers, and other risks.

### **Cloud Access Security Broker (CASB)**

CASB solutions provide security to **Software-as-a-Service (SaaS)** applications, users, and data. Inline CASB offers visibility, compliance, and threat protection for data in motion and at rest in cloud apps, but also creates shadow IT reports, risk assessments, and more.

## **Types of Firewalls**

A cloud firewall, also known as firewall-as-a-service **(FWaaS),** is a security service hosted in the cloud that filters network traffic to protect cloud resources, similar to how traditional firewalls protect on-premise networks. It offers scalability and simplified management compared to traditional hardware-based firewalls.

# **Osi Model And Network Security**

### **OSI MODEL Layers:**

1. Application,  
2. Presentation,  
3. Session,  
4. Transport,  
5. Network,  
6. Data Link,  
7. Physical

Each of the seven OSI model layers communicates with layers below and above it. For example, the application layer interacts with software applications, while the presentation layer provides encryption and data compression. Likewise, the session layer creates communications between devices. The transport layer breaks data into chunks (called segments) to send them, then the receiving device reassembles the segments before the network layer breaks them into smaller packets to send to other networks. The data link layer facilitates data transfer between devices on the same network, and, finally, the physical layer transfers data in machine language (ones and zeros).

# **Why Network Security Matters**

## **Why Network Security Matters**

Digitization has transformed our world. How we live, work, play, and learn have all changed. Every organization that wants to deliver the services that customers and employees demand must protect its network. Network security also helps you protect proprietary information from attack. Ultimately it protects your reputation.

# **Detailed Study of Network-Based Security Threats**

## **Study of Network-Based Security Threats**

Network security threats refer to any potential danger to network integrity, confidentiality, or availability. These threats manifest as unauthorized access attempts, data interception, service disruptions, or infrastructure compromises.

Threat actors range from opportunistic cybercriminals to state-sponsored groups with advanced capabilities and resources.

Modern network security threats and solutions have evolved significantly. Today's sophisticated attacks often combine multiple techniques, leverage automation, and exploit both technical vulnerabilities and human psychology.

According to IBM's 2024 Cost of a Data Breach Report, data breaches remain extremely costly security incidents, with organizations spending an average of $4.88 million per breach, a 10% increase from 2023 figures.

The consequences of successful network security attacks extend far beyond immediate financial losses. Organizations face regulatory penalties, legal liabilities, reputational damage, operational disruptions, and erosion of customer trust.

A single network breach can undermine years of business development efforts.

Network security is now a core business need, not just a technical issue. Several key factors drive this increased importance.

* **Expanding Attack Surface:** The proliferation of cloud services, remote work arrangements, IoT devices, and mobile computing has dramatically expanded network boundaries. According to the Futurum Group's 2024 research on multi-cloud management, 78% of organizations now manage hybrid environments that blend on-premises, multi-cloud, and edge computing resources.  
* **Regulatory Requirements:** Stringent data protection regulations like GDPR, CCPA, and industry-specific frameworks impose substantial obligations around network risk management. Non-compliance can result in a large sum of financial penalties and mandatory breach disclosure requirements.  
* **Economic Incentives:** Cybercrime has become a lucrative industry. Ransomware attacks are projected to cost victims $57 billion this year, according to Cybersecurity Ventures. This figure is expected to grow dramatically to $275 billion by 2031\. Such enormous financial impacts create powerful incentives for threat actors to continuously refine their attack methods.  
* **Operational Continuity:** As business processes become increasingly digitized, network disruptions directly impact core operations. The average cost of network downtime can be thousands of dollars per minute, even for mid-sized enterprises.

## **Network-Based Security Threats (Case Studies)**

### **Telecom Provider Infiltration (November 2024\)**

Chinese hackers known as "Salt Typhoon" breached at least eight U.S. telecommunications providers and telecom companies in over twenty other countries. The attackers established persistent network access for up to two years. They created covert proxy networks that allowed them to steal customer call data and law enforcement surveillance request information.

### **Russian Military Network Attack (January 2025\)**

Russian hackers executed spear phishing attacks against Kazakh diplomatic entities by embedding malicious code within diplomatic documents. The attackers specifically targeted network infrastructure to establish persistent access, enabling long-term cyber espionage activities through compromised network gateways and routing tables.

### **Romanian Election Infrastructure (December 2024\)**

Russian hackers targeted Romania's election systems with over 85,000 network-based attacks before the country's presidential vote. The campaign included network penetration attempts and credential theft. Attackers posted stolen network authentication credentials on Russian hacker forums while maintaining persistent network probing throughout election day.

### **Ukrainian Defense Network Targeting (December 2024\)**

Russian hackers launched a sophisticated phishing campaign targeting Ukrainian armed forces' defense networks and military communication infrastructure. The attackers deployed specialized remote access tools designed to infiltrate military networks and steal credentials from secure communication platforms and local area networks.

### **Network Attacks on Taiwan (January 2025\)**

Chinese groups launched 2.4 million daily hacking attempts against Taiwan in 2024\. They targeted government systems and telecom networks. These attacks used advanced persistent threat (APT) techniques. Hackers bypassed security controls and created hidden communication channels.

### **European Government Network Breaches (May 2024\)**

Polish and Czech officials reported that Russian cyber spies targeted their government and infrastructure networks using a Microsoft Outlook vulnerability as the initial access vector. The attackers established a persistent network presence to exfiltrate sensitive government communications and intelligence data.

### **Italian Government DDoS Campaign (January 2025\)**

A pro-Russian hacking group executed a coordinated denial-of-service attack targeting Italian government websites, including ministries, public services, and transportation platforms across major cities. The attackers flooded the network infrastructure with malicious traffic, rendering critical government network resources inaccessible during periods of heightened diplomatic tension.

### **Microsoft Corporate Network Breach (January 2024\)**

Russian hackers broke into Microsoft's networks using password spray attacks. These attacks targeted login systems with repeated attempts. The hackers gained access to areas with senior leadership messages. They also breached security monitoring systems and secret document storage.  
