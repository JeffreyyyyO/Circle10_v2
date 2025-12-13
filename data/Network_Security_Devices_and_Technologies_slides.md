# **Month 1, Week 2:**

# **Network Security Devices and Technologies**

### **Overview**

* Firewalls  
* Intrusion Detection and Prevention System  
* Virtual Private Network  
* Secure Network Design Principles  
* Advanced Threat Protection

# **Network Security Devices & Technologies**

## **Firewalls**

Firewalls are essential security tools that act as a barrier between a trusted network and untrusted networks, like the internet. They filter network traffic based on predefined rules, preventing unauthorized access and malicious attacks.

## **Types of Firewalls**

### **Packet Filtering Firewalls**

**Packet filtering firewalls** examine network traffic by inspecting packet headers and applying predefined rules to determine whether to allow or block the traffic. These rules are based on factors like the source and destination IP addresses, protocol type, and port number. By controlling which packets are allowed through, these firewalls enhance network security by preventing unauthorized access and managing traffic flow.

### **Stateful Inspection Firewalls**

**Stateful inspection firewalls** are network security devices that monitor and track the state of active network connections to make more informed decisions about allowing or blocking traffic. They go beyond simple packet filtering by tracking connection details like IP addresses, port numbers, and sequence numbers, ensuring that only legitimate and expected traffic is permitted.

### **Application-Level Gateways**

**Application-level gateways**, also known as proxy firewalls, act as intermediaries between a network and the internet, handling specific application protocols to enhance security. Unlike packet-filtering firewalls, they operate at a higher level, inspecting and controlling traffic based on application data, offering deeper content inspection and the ability to mask internal IP addresses.

### **Next-Generation Firewalls (NGFWs)**

**Next-Generation Firewalls (NGFWs)** are advanced network security devices that combine traditional firewall capabilities with additional features to protect against modern cyber threats. They go beyond basic packet filtering by inspecting traffic at deeper levels, identifying applications, users, and content to enforce granular security policies. NGFWs integrate capabilities like intrusion prevention systems (IPS), application awareness, and threat intelligence to provide a more robust defense against sophisticated attacks.

### **Hardware Firewalls**

**Hardware firewalls** are physical devices that protect entire networks by filtering network traffic based on predefined rules, acting as a security barrier between the internet and your internal network. They analyze data packets and block those deemed malicious, while allowing safe traffic to pass through. These devices are distinct from software firewalls, which protect individual devices, and are often used in conjunction with software firewalls for comprehensive security.

### **Software Firewalls**

A **software firewall** is a program installed on a computer or server that monitors and controls network traffic to and from that device. It acts as a barrier, inspecting data packets and blocking those that don't match predefined security rules. Essentially, it provides a layer of security at the software level, protecting against unauthorized access and malicious software.

### **Cloud Firewalls**

A **cloud firewall**, also known as firewall-as-a-service (FWaaS), is a security service hosted in the cloud that filters network traffic to protect cloud resources, similar to how traditional firewalls protect on-premise networks. It offers scalability and simplified management compared to traditional hardware-based firewalls.

# **Intrusion Detection & Prevention System**

## **Intrusion Detection & Prevention System (IDPS)**

An **intrusion detection and prevention system (IDPS)** is a solution that monitors a network for threats and then takes action to stop any threats that are detected.

An IDPS is closely related to an intrusion detection system (IDS). While both systems detect threats and send alerts about them, an IDPS also attempts to remediate those threats.

An IDPS is sometimes called an intrusion prevention system (IPS). The terms IDPS and IPS are mostly used interchangeably, but when someone mentions an IPS they are often referring to the threat hunting function of an IDPS.

An IDPS works in several different ways depending on the vendor, the chosen deployment method, and the needs of the organization deploying it.

## **Types of Intrusion Detection & Prevention Systems**

### **Network-based IDPS**

**Network-based IDPS (NIPS)** is a type of IDPS installed at specific points within a network to monitor all of that network's traffic and scan for threats. The NIPS often does this by analyzing activity and matching it against a database of known attacks configured manually by a security expert. If the activity matches a known threat in the database, it isn't allowed to proceed through the network. A NIPS is often deployed at the boundaries of networks, such as in routers or modems, behind firewalls, and at network remote access points.

* **Wireless intrusion prevention systems (WIPS)** monitor wireless networks for the presence of rogue access points and unrecognized devices by analyzing the network's radio frequencies. WIPS are deployed in wireless networks and in places that are vulnerable to unauthorized wireless access.

* **Network behavior analysis (NBA)** systems check network traffic for unusual patterns of activity. For example, in a distributed denial of service attack (DDoS), thousands of requests are sent to the network to overwhelm it. Any of these requests alone might look valid, but together illustrate a problem. NBA systems often reinforce a more standard NIPS in an organization's internal networks.

### **Host-based IDPS**

**Host-based IDPS (HIPS)** are deployed on a single host-often a key server with critical data-or public servers that are gateways to an organization's internal network. A HIPS specifically monitors traffic flow on its host system. HIPS are generally set to detect host operating system activity and internet protocol suite (TCP/IP) activity.

**Detection methods:** Once in place, an IDPS uses a variety of techniques to identify threats. These techniques broadly fall into 3 categories:

* **Signature-based threat detection** matches monitored activity to a database filled with signatures-a unique pattern or identifier-of previously identified threats. While this method is good at detecting well-known threats, novel threats will go undetected.

* **Anomaly-based threat detection** matches a random selection of network activity against a baseline standard of network activity. If the random selection is different enough from the baseline, then the threat triggers action. While this detection method captures novel threats, it also creates more false positives than signature-based threat detection. Anomaly-based threat detection is the aspect of IDPS that is most enhanced by advancements in artificial intelligence and machine learning algorithms.

* **Protocol-based (or policy-based) threat detection** is similar to signature-based threat detection, but it uses a database of specific protocols defined by the organization and blocks any activity violates those protocols. The protocols must be manually configured by a security expert.

## **IDPS Actions**

### **Prevention actions**

Once the IDPS detects a perceived threat, it can take several courses of action-depending on how it's set up and the type of threat detected. Common preventative actions against attacks are to:

1. **Alert administrators**

   In this most basic type of response, the IDPS alerts human security administrators, much like an intrusion detection system would. Alerts like this are created when an automatic action might not be appropriate, or when the system is unsure if there is a false positive.

2. **Employ banishment vigilance**

   When the IDPS takes this action, it stops incidents before they have a chance to occur by blocking traffic or flagged users from a threatening IP address. A common example is blocking an IP address that has failed a password check too many times.

3. **Change the security environment**

   Similar to banishment vigilance, this technique has the IDPS change the security setup of the network to prevent the threat from gaining access. An example of this response would be reconfiguring a firewall.

4. **Modify the attack content**

   This technique involves automatically altering the content of the attack. For example, if a suspicious email is flagged, the IDPS would remove any aspect of the email that might contain content malicious to the network, such as email attachments.

# **Virtual Private Network**

## **Virtual Private Network (VPN)**

A **virtual private network (VPN)** provides a secure, encrypted connection over a public network like the internet, enabling users to access private networks and resources remotely while maintaining privacy and security. It essentially creates a "tunnel" for your internet traffic, masking your IP address and encrypting your data to protect it from potential eavesdroppers and trackers.

## **Virtual Private Network Actions**

* **Encrypts your internet traffic:** VPNs use encryption protocols to scramble your data, making it unreadable to anyone who might intercept it.  
* **Masks your IP address:** By routing your traffic through a VPN server, your real IP address is hidden, and the VPN server's IP address is used instead.  
* **Provides secure remote access:** VPNs allow remote users to connect to a private network, like a company's internal network, as if they were physically connected to it.  
* **Bypasses geographical restrictions:** VPNs can be used to access content that is blocked in certain regions by connecting through a server in a different location.

## **Types of Virtual Private Networks**

* **Remote access VPN:** Allows individual users to securely connect to a private network from remote locations.  
* **Site-to-site VPN:** Connects two or more geographically separated networks together, creating a single, larger private network.

# **Advanced Threat Protection**

## **Advanced Threat Protection**

**Advanced Threat Protection (ATP)** refers to security solutions and technologies designed to detect, prevent, and respond to sophisticated cyberattacks that bypass traditional security measures. These attacks often involve malware, phishing, ransomware, and advanced persistent threats (APTs). ATP solutions use a combination of techniques like behavioral analysis, machine learning, and threat intelligence to identify and neutralize threats, offering a more comprehensive defense than traditional security systems.

## **Advanced Threat Protection Actions**

Advanced Threat Protection protects against the following:

* **Malware:** ATP solutions can detect and block various forms of malware, including ransomware, viruses, and Trojans.  
* **Phishing attacks:** ATP can identify and block phishing attempts, which aim to steal user credentials or other sensitive information.  
* **Business Email Compromise (BEC):** ATP helps prevent BEC scams, where attackers impersonate legitimate business contacts to trick recipients into sending money or sensitive data.  
* **Targeted attacks:** ATP solutions are designed to detect and respond to targeted attacks aimed at specific individuals or organizations.  
* **Zero-day threats:** ATP can protect against zero-day exploits, which are vulnerabilities not yet known to the vendor or security community.

Advanced Threat Protection carries out the following actions:

* **Sandboxing:** ATP solutions often use sandboxing to analyze suspicious files in a safe, isolated environment to observe their behavior before allowing them to run.  
* **Behavioral analysis:** ATP monitors system behavior for anomalies that could indicate malicious activity, such as unusual network connections or file access patterns.  
* **Threat intelligence:** ATP solutions leverage threat intelligence feeds, which provide information about known threats and attack techniques, to enhance their detection capabilities.  
* **Machine learning and AI:** Many ATP solutions use machine learning and artificial intelligence to identify new and evolving threats, adapt to changing attack patterns, and automate threat detection and response.  
* **Network traffic analysis:** ATP solutions analyze network traffic for suspicious patterns and malicious activity.  
* **Cloud security:** ATP can protect cloud-based environments and applications from advanced threats.  
* **Endpoint security:** ATP can protect individual devices (endpoints) from malware and other threats.  
* **Email security:** ATP can scan emails and attachments for malicious content and block or quarantine suspicious messages.