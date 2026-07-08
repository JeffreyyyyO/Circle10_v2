# **CYS 532—WEEK 3—LAB**

## **SYSTEMS SECURITY**

# **Configuring Iptables for Network Access Control**

## **Overview & Objectives**

This lab series provides step-by-step instructions on securing a Linux server using iptables—the native netfilter-based command-line utility for configuring system firewalls. Over the course of these labs, you will transform a default, wide-open network state into a hardened environment using the **Default Deny Inbound** principle.

### **Lab Objectives:**

* Analyze and reset running firewall structures.  
* Establish a default-deny security baseline.  
* Implement stateful packet inspection to allow established connections.  
* Restrict administrative interfaces (SSH) to specific, trusted hosts.  
* Open public web services (HTTP/HTTPS) to all interfaces.  
* Implement active logging, rate-limiting SSH protection, and automatic log-scanning using Fail2ban.

## **Lab 1: Resetting & Establishing the Default Firewall Posture**

### **1.1 Overview and Objectives**

In this lab, you will check the existing firewall state of your server, flush any conflicting configuration profiles, and establish a foundational, highly secure **Default Deny Inbound** firewall posture.  
At the end of this lab, you will understand:

1. The structural roles of the core built-in packet chains (INPUT, FORWARD, OUTPUT).  
2. How default policy overrides function in netfilter.  
3. How to safely secure a server's interfaces without restricting outbound capabilities.

### **1.2 Lab Prerequisites**

To complete this lab, you need the following environment:

* **Operating System:** A Linux environment based on **Debian/Ubuntu** or **CentOS/RHEL** (Kali Linux is also suitable and comes pre-installed with the required packages).  
* **Privileges:** sudo administrative access or direct system root privileges.  
* **Core Utility:** The iptables package.

### **1.3 Step 1: Checking and Analyzing Existing Rules**

Before modifying packet rules, you must inspect the system's live netfilter table configuration.  
Run the following command to list active rules in a verbose, numeric format:  
sudo iptables \-nvL

#### **Syntax Breakdown:**

* sudo: Executes the command with system-administrator privileges.  
* iptables: Invokes the default kernel packet filtering configuration tool.  
* \-L (or \--list): Lists the rules in all active chains.  
* \-n (or \--numeric): Displays IP addresses and port numbers in numeric format. This prevents slow reverse-DNS lookups, making command execution instantaneous.  
* \-v (or \--verbose): Displays additional packet counters, byte sizes, active interfaces, and target options.

#### **Expected Default Output (Clean Server):**

If your firewall has not been modified or was recently reset, you will see a listing of empty default chains:  
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)  
 pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)  
 pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)  
 pkts bytes target     prot opt in     out     source               destination         

### **1.4 Understanding the Default Chains**

The netfilter architecture filters traffic based on where a packet is headed. Below is a structural breakdown of the default chains displayed in the output:

| Chain Name | Purpose | Default Risk / Standard Behavior |
| :---- | :---- | :---- |
| **INPUT** | Handles all network packets destined directly for your local server (incoming connections like SSH, HTTP requests). | **Critical Risk:** By default, this policy is set to ACCEPT. Any machine on the network can attempt access. |
| **FORWARD** | Handles packets that pass *through* your server to another network interface (routing, bridging, Docker virtualization). | Often set to DROP on servers to prevent unauthorized packet forwarding, but handled by Docker/VPN services if required. |
| **OUTPUT** | Handles outbound network packets generated natively by your server (updating system packages, resolving DNS, outbound API calls). | Safely left as ACCEPT to allow the server to operate on the internet normally. |

*Note: If Docker is running on your host, you may also see custom Docker-created chains (e.g., DOCKER, DOCKER-ISOLATION-STAGE-1, DOCKER-USER). These are automatically managed by the Docker engine to containerize bridge networks.*

### **1.5 Step 2: Flushing Active Rules**

To ensure a clean starting state and remove any lingering custom configurations, flush all existing rules in the current table using the \-F flag:  
sudo iptables \-F

**Warning:** Running iptables \-F removes all rules but **does not change the default policies** of the chains. If your default policies are set to DROP and you flush your active access rules, you may lock yourself out of your remote system immediately. Always ensure default policies are set to ACCEPT or you have a stateful exception rule before flushing on remote cloud servers.

### **1.6 Step 3: Implementing the Default-Deny Posture**

The core principle of modern firewall security is **Default Deny**. We want to reject all incoming and forwarding connection attempts by default, and only allow traffic that we explicitly define as safe.  
Execute the following commands to configure default policies:

#### **A. Set the Default Incoming Policy to DROP**

This drops any incoming packet trying to reach your server unless a specific exception rule allows it.  
sudo iptables \-P INPUT DROP

#### **B. Set the Default Forwarding Policy to DROP**

This ensures your machine does not act as an unapproved router or bridge.  
sudo iptables \-P FORWARD DROP

#### **C. Set the Default Outgoing Policy to ACCEPT**

This allows your machine to send data out freely (e.g., installing software updates, loading web pages).  
sudo iptables \-P OUTPUT ACCEPT

#### **Syntax Breakdown:**

* \-P (or \--policy): Modifies the default target policy of a built-in chain.  
* DROP: Silently discards any packet that reaches the end of the chain without hitting an explicit match.  
* ACCEPT: Allows packets to pass through by default.

### **1.7 Step 4: Verification**

Confirm that your baseline default-deny policy has been correctly written to the kernel table:  
sudo iptables \-nvL

#### **Output Verification:**

Observe the updated **policy** keywords next to each chain name:  
Chain INPUT (policy DROP 0 packets, 0 bytes)  
...  
Chain FORWARD (policy DROP 0 packets, 0 bytes)  
...  
Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)  
...

Your server is now locked down. Any host attempting to ping, scan, or log into this server will receive no response.

### **Progress Checkpoint**

* Inspected current iptables configuration using \-nvL.  
* Safely initialized tables with the \-F flush command.  
* Modified baseline chain parameters to block all unsolicited inbound/forwarding data.  
* Maintained outgoing network functionality for standard operations.

## **Lab 2: Stateful Exception Tracking & Port Security Rules**

### **2.1 Overview and Objectives**

In Lab 1, you completely shut down incoming traffic to your server. In this lab, you will strategically construct rules to allow the server to communicate safely. You will establish stateful packet inspection, restrict administrative ports (SSH) to a trusted workstation IP address, open standard web ports to the general public, and log unauthorized access attempts.  
At the end of this lab, you will understand:

1. The difference between stateless and stateful connection tracking in netfilter.  
2. How to create source-restricted transport layer (TCP) rules.  
3. The absolute importance of **rule sequence** in firewall chains.

### **2.2 Step 1: Permitting Established and Related Outbound Sessions**

When your server initiates a connection with an external system (e.g., retrieving a software update with apt or querying a remote API), the remote system must send packets back to your server. Because you set your default inbound (INPUT) policy to DROP in Lab 1, those return packets would be discarded.  
To prevent this, you must instruct iptables to keep track of outbound connections and automatically permit returned traffic.  
Run the following stateful tracking command:  
sudo iptables \-A INPUT \-m conntrack \--ctstate ESTABLISHED,RELATED \-j ACCEPT

💡 **Educational Note:** Older legacy tutorials often recommend sudo iptables \-A INPUT \-m state \--state ESTABLISHED,RELATED \-j ACCEPT. While this command is still understood, the state module has been superseded by the newer conntrack (connection tracking) module (-m conntrack \--ctstate). The conntrack module is highly optimized and handles complex protocols more securely.

#### **Syntax Breakdown:**

* \-A INPUT: Appends the rule to the end of the INPUT chain.  
* \-m conntrack: Invokes the stateful connection tracking kernel extension.  
* \--ctstate ESTABLISHED,RELATED: Matches packets belonging to:  
  * ESTABLISHED: Connections that have already successfully negotiated two-way communication.  
  * RELATED: Packets that are starting new connections but are linked to an existing active session (such as passive FTP data channels or ICMP errors).  
* \-j ACCEPT: Directly accepts matching packets, bypassing any downstream restrictions.

### **2.3 Step 2: Restricting SSH Access to a Specific Trusted Workstation**

Leaving administrative services like SSH open to the whole internet invites constant brute-force attacks. You will construct a rule that permits SSH (TCP Port 22\) connections **only** if they originate from a designated trusted workstation IP address: 192.168.100.100.  
Run the following rule:  
sudo iptables \-A INPUT \-p tcp \-s 192.168.100.100 \--dport 22 \-j ACCEPT

#### **Syntax Breakdown:**

* \-p tcp: Limits the scope of this rule exclusively to TCP transport protocol packets.  
* \-s 192.168.100.100: Limits the rule's match criteria to the specified workstation's source IP address.  
* \--dport 22: Matches packets targeted to local port 22 (the standard service port for SSH).  
* \-j ACCEPT: Permits the matching packets to connect.

### **2.4 Step 3: Permitting Public HTTP and HTTPS Web Traffic**

In contrast to private administrative access, public web services must remain accessible to all systems. You will add rules to permit global traffic on standard unencrypted (HTTP Port 80\) and secure encrypted (HTTPS Port 443\) protocols from any source.  
Run the following rules:  
\# Allow HTTP Access  
sudo iptables \-A INPUT \-p tcp \--dport 80 \-j ACCEPT

\# Allow HTTPS Access  
sudo iptables \-A INPUT \-p tcp \--dport 443 \-j ACCEPT

#### **Syntax Breakdown:**

* Because no source IP option (-s) is defined, iptables implicitly matches any source address (0.0.0.0/0), allowing the entire internet to reach your web ports.

### **2.5 Step 4: Configuring Logging and Finalizing the Drop Policy**

To understand what traffic is probing your system, you must log dropped packets. In netfilter, logging acts as a non-terminating target; a matched packet is written to system logs but continues down the chain to be processed by subsequent rules.  
⚠️ **Critical Architectural Rule (Sequence Trap):** The order of rules is strictly sequential. If you place a catch-all DROP rule above a LOG rule, the packet will terminate on the DROP target and the LOG rule will never execute.  
Therefore, you must write your LOG rule **first**, and your catch-all DROP rule **second**:

#### **A. Append the Logging Rule**

sudo iptables \-A INPUT \-j LOG \--log-prefix "iptables-dropped: " \--log-level 4

#### **Syntax Breakdown:**

* \-j LOG: Sets the target action to write the matched packet metadata directly to the system kernel log buffer (dmesg / syslog).  
* \--log-prefix "iptables-dropped: ": Prepends a custom search string (up to 29 characters) to every log line, making audit parsing highly efficient.  
* \--log-level 4: Sets the logging severity to "Warning". This ensures events are captured clearly without filling system logs with debug noise.

#### **B. Append the Catch-All Drop Rule**

sudo iptables \-A INPUT \-j DROP

This rule acts as a final safety net, capturing and silently discarding any packet that was not explicitly matched by your established-connection, trusted-SSH, or public-web rules.

### **Progress Checkpoint**

* \[x\] Implemented stateful tracking (conntrack) to allow returned outbound sessions.  
* \[x\] Isolated administrative SSH (Port 22\) to source host 192.168.100.100.  
* \[x\] Configured unrestricted global public HTTP (80) and HTTPS (443) ingress.  
* \[x\] Configured structured packet logging with clear prefix labels.  
* \[x\] Built a sequential firewall terminate-upon-drop mechanism.

## **Lab 3: Log Monitoring, Rate Limiting, Persistence, & Intrusion Prevention**

### **3.1 Overview and Objectives**

Implementing firewall rules is only half of network security; you must also monitor network activity and dynamically adapt to threats. In this lab, you will audit your custom packet logs, implement a dynamic rate-limiting SSH protection rule directly inside iptables, configure rules to persist across system reboots, and integrate Fail2ban to dynamically scan system logs and automate host blocking.  
At the end of this lab, you will understand:

1. How to read and parse network logs in Linux.  
2. How to implement basic stateful rate-limiting without third-party software using the recent module.  
3. How to orchestrate a defense-in-depth posture using iptables and Fail2ban.

### **3.2 Step 1: Auditing Your Firewall Logs in Real-Time**

Because you appended a custom LOG rule in Lab 2, unauthorized access attempts will show up in your system logs. You can audit these events in real-time using journalctl.  
Run the following monitoring command:  
sudo journalctl \-kf | grep "iptables-dropped"

#### **Syntax Breakdown:**

* journalctl: Systemd utility to query the system log journal.  
* \-k (or \--dmesg): Filters results to show only system kernel messages.  
* \-f (or \--follow): Follows the log output in real-time, showing new entries as they arrive.  
* grep "iptables-dropped": Isolates lines matching the custom prefix you created in Lab 2\.

### **3.3 Step 2: Implementing Dynamic SSH Rate-Limiting**

Brute-force attacks automate SSH logins to guess system credentials. To defend your system, you can use the recent extension module in iptables. This module dynamically tracks IP addresses that connect to a port and lets you block them if they exceed a specific connection rate.  
You will configure a rate limiter that permits an IP address a maximum of **30 connections per 60 seconds** on TCP Port 22\. If an IP exceeds this rate, its subsequent connections are dropped automatically.

#### **A. Create the Tracking Rule**

First, register incoming connections to Port 22 in a dynamic tracking list:  
sudo iptables \-A INPUT \-p tcp \--dport 22 \-m recent \--set \--name SSH\_BRUTE

#### **Syntax Breakdown:**

* \-m recent: Invokes the stateful tracker module.  
* \--set: Adds the source IP of the incoming packet to the tracking database list.  
* \--name SSH\_BRUTE: Sets the name of the dynamic tracking list to SSH\_BRUTE.

#### **B. Implement the Rate Enforcement Rule**

Next, restrict connection attempts by checking if the source IP is on the list and has exceeded the threshold. This rule must be evaluated **before** the standard acceptance rule. Therefore, insert it at the top of your chain using the \-I flag:  
sudo iptables \-I INPUT 1 \-p tcp \--dport 22 \-m recent \--update \--seconds 60 \--hitcount 30 \--name SSH\_BRUTE \-j DROP

#### **Syntax Breakdown:**

* \-I INPUT 1: Inserts this rule at index 1 (the very top) of the INPUT chain, ensuring it is evaluated before any general acceptance rules.  
* \--update: Checks if the source IP address is already registered in the SSH\_BRUTE list and updates its activity timestamp.  
* \--seconds 60: Limits the check to connection attempts made in the last 60 seconds.  
* \--hitcount 30: Triggers the target action if the IP address is detected 30 or more times within the 60-second window.  
* \-j DROP: Silently discards the packet, preventing the brute-force attempt.

### **3.4 Step 3: Making Your Rules Persistent Across Reboots**

By default, iptables rules exist only in system memory. If your server restarts, your rules will be lost. To prevent this, you must write them to disk so they can be reloaded automatically during system boot.

#### **For Debian and Ubuntu Systems:**

1. Install the utility package that handles netfilter rule persistence:  
   sudo apt update && sudo apt install \-y iptables-persistent

   *During installation, a prompt will ask if you want to save current IPv4 and IPv6 rules. Select **Yes**.*  
2. If you update your rules later, save the active configuration manually:  
   sudo netfilter-persistent save

#### **For CentOS and Red Hat Enterprise Linux Systems:**

1. Save your current memory structures directly to the default firewall configuration path:  
   sudo service iptables save

   *(Or write the memory structures directly to disk):*  
   sudo iptables-save | sudo tee /etc/sysconfig/iptables

### **3.5 Step 4: Integrating Fail2ban for Automated Intrusion Prevention**

While iptables provides excellent low-level control, managing complex log analysis manually can be tedious. To add an extra layer of defense, you can integrate **Fail2ban**.  
Fail2ban is a background service that scans system logs (such as /var/log/auth.log or /var/log/secure) for suspicious activity, such as repeated failed login attempts. If it detects a malicious host, Fail2ban dynamically injects temporary iptables reject rules to block that host.

#### **A. Install Fail2ban**

sudo apt install \-y fail2ban

#### **B. Enable and Start the Service**

Configure Fail2ban to start automatically during system boot and run immediately:  
\# Configure startup service  
sudo systemctl enable fail2ban

\# Start the daemon  
sudo systemctl start fail2ban

#### **C. Monitor Fail2ban Status and Active Jails**

Fail2ban organizes service monitors into "jails." By default, the SSH daemon monitoring jail (sshd) is enabled upon installation.  
Verify the health and active monitoring status of Fail2ban:  
sudo fail2ban-client status

#### **Expected Output:**

Status  
|- Number of jail:      1  
\`- Jail list:           sshd

#### **D. Inspect the SSH Monitoring Jail**

To check which IP addresses are currently blocked by Fail2ban, query the specific sshd jail:  
sudo fail2ban-client status sshd

#### **Expected Output:**

Status for the jail: sshd  
|- Filter  
|  |- Currently failed: 1  
|  |- Total failed:     5  
|  \`- File list:        /var/log/auth.log  
\`- Actions  
   |- Currently banned: 1  
   |- Total banned:     1  
   \`- Banned IP list:   198.51.100.50

If an IP address tries to guess your password repeatedly, Fail2ban will automatically detect the failed logins in your system log, call the iptables command behind the scenes, and ban that host for a set period.

### **Progress Checkpoint**

* Monitored live packet drop logs using journalctl.  
* Configured stateful dynamic rate limiting to block brute-force attempts on Port 22\.  
* Saved rule configurations to disk for persistence across system reboots.  
* Integrated and verified Fail2ban to automate intrusion prevention.

## **Final Review: Comprehensive Lab Ruleset Architecture**

After completing all three labs, your running system's iptables configuration will match this secure architecture:  
\[ Incoming Traffic Ingress \]  
             |  
             v  
\+------------------------------------------------------------+  
| 1\. Dynamic SSH Rate Check (recent)                         | \---\> If \> 30 hits/60s: \[ DROP \]  
\+------------------------------------------------------------+  
             | Pass  
             v  
\+------------------------------------------------------------+  
| 2\. Stateful conntrack (ESTABLISHED, RELATED)               | \---\> \[ ACCEPT \] (Allow Return Traffic)  
\+------------------------------------------------------------+  
             | No Match  
             v  
\+------------------------------------------------------------+  
| 3\. Restricted Ingress (TCP Port 22 Source: 192.168.100.100) | \---\> \[ ACCEPT \] (Trusted Admin Only)  
\+------------------------------------------------------------+  
             | No Match  
             v  
\+------------------------------------------------------------+  
| 4\. Public Web Port Ingress (TCP Port 80 & 443\)             | \---\> \[ ACCEPT \] (Global Access)  
\+------------------------------------------------------------+  
             | No Match  
             v  
\+------------------------------------------------------------+  
| 5\. Register IP under brute list (recent \--set)             | \---\> (Passive Tracking)  
\+------------------------------------------------------------+  
             | Proceed  
             v  
\+------------------------------------------------------------+  
| 6\. Custom Logging (prefix "iptables-dropped:")             | \---\> (Logged to Kernel Log Buffer)  
\+------------------------------------------------------------+  
             | Proceed  
             v  
\+------------------------------------------------------------+  
| 7\. Catch-All Baseline Drop Policy                          | \---\> \[ DROP \] (Discard All Else)  
\+------------------------------------------------------------+

This multi-layered approach gives you full control over who can access your server, protecting it from attacks while keeping your services running smoothly.

