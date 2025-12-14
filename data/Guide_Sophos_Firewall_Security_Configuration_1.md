# **Guide: Sophos Firewall Security Configuration 1**

# **Sophos Firewall (SFOS) Configuration of 3 Security Policies**

This guide provides step-by-step instructions for configuring three key security policies on your Sophos Firewall and procedures for testing them within your VMware Workstation Pro environment.

## **Prerequisites**

1. **Network Setup:** All client VMs (Kali, Parrot, Ubuntu, Windows Server 2022\) must be connected to a network interface (e.g., a custom vSwitch or VLAN) that is protected by the Sophos Firewall. The Sophos Firewall must have a working LAN and WAN (or simulated WAN) interface and should be configured as the default gateway and DNS server for the client VMs.  
2. **Access:** You must be able to access the Sophos Firewall web console from one of your client VMs (e.g., Windows Server 2022 or Ubuntu).

## **1\. Threat Protection Policy (Denial-of-Service Prevention)**

Denial-of-Service (DoS) and Distributed Denial-of-Service (DDoS) protection is usually configured under the Intrusion Prevention System (IPS) settings, specifically the "DoS & Spoof Protection" module, where you set rate limits for various flood attacks.

### **Configuration Steps**

1. **Access DoS Settings:**  
   * Log into the Sophos Firewall web console.  
   * Navigate to **Protect** (or **Intrusion Prevention**) \> **DoS & spoof protection**.  
2. **Configure Flood Protection:**  
   * Locate the **DoS settings** section.  
   * You will see settings for different flood types (TCP SYN Flood, UDP Flood, ICMP Flood). These settings limit the number of packets per second (PPS) allowed for a specific connection type (source or destination).  
   * **TCP SYN Flood Protection:** This is critical for preventing common web server DoS attacks.  
     * **Mode:** Select Source and destination addresses (Recommended default).  
     * **Source Packet Rate (PPS):** Set a reasonable limit based on your network traffic (e.g., 2000-5000 PPS). This limits the requests *from* a source.  
     * **Destination Packet Rate (PPS):** Set a limit for packets *to* a destination (e.g., 500 PPS if you are protecting a web server).  
   * **UDP Flood Protection:** Configure similar rate limits for UDP traffic.  
   * **ICMP Flood Protection:** Configure similar rate limits for ICMP (ping) traffic.  
   * **Logging:** Ensure logging is set to at least Limited or Everything to capture attack attempts.  
   * Click **Apply** or **Save**.  
3. **Enable Spoof Prevention (Recommended):**  
   * Under **Spoof protection**, select Enable spoof prevention.  
   * Select the zones you want to protect (e.g., **LAN**).  
   * Click **Apply** to prevent IP address forgery.

### **Testing Procedure**

The goal is to generate traffic that exceeds the configured ICMP or SYN limits from a client VM (e.g., Kali or Parrot) and observe the firewall's action.

1. **Identify Target:** Choose a target IP address (e.g., the Windows Server 2022 VM, or a simulated external resource/WAN address).  
2. **Use Hping3 (Kali/Parrot):** Use the hping3 tool (pre-installed on Kali/Parrot) to send a SYN flood attack.  
   * **Syntax:** sudo hping3 \-S \--flood \-p 80 \<Target\_IP\_Address\>  
     * \-S: SYN flag set  
     * \--flood: Send packets as fast as possible, exceeding any reasonable rate limit.  
     * \-p 80: Target port 80 (HTTP).  
3. **Monitor Sophos Logs:**  
   * Immediately go to the Sophos Firewall web console and navigate to **Monitor & Analyze** \> **Log Viewer**.  
   * Filter the Log Component by **Intrusion Prevention** or **System**.  
   * You should see entries indicating that the firewall is dropping packets, logging an **ARP/SYN Flood Attack Attempt**, and potentially blocking the source IP (your Kali/Parrot VM) temporarily due to rate limiting.  
4. **Verify Service Status (Windows Server):** After running the attack, try to access a service (like a web page) on the Windows Server VM from another client VM (e.g., Ubuntu). If the attack was successful, the Windows Server service should be unaffected, and the Kali traffic should be blocked.

## **2\. Content Filtering Security Policy (Block News Websites)**

This involves creating a **Web Policy** that blocks the pre-defined News and Media category and then applying this policy to your LAN-to-WAN firewall rule.

### **Configuration Steps**

1. **Check/Create Web Policy:**  
   * Navigate to **Protect** \> **Web** \> **Policies**.  
   * You can either edit an existing policy (e.g., the Default Policy) or click **Add Policy** to create a new one, named Block\_News\_Policy.  
2. **Add a Block Rule:**  
   * Select your chosen policy and click **Add Rule**.  
   * **Rule Name:** Block News Category  
   * **For Users/Groups:** Select Any (or specific groups if desired).  
   * **Activities:**  
     * Click Add New Item.  
     * In the pop-up, click **Show** and select **Web Category**.  
     * Search for and select the category: **News and Media**.  
     * Click **Apply**.  
   * **Action:** Change the action to **Block**.  
   * **Save** the rule and then **Save** the policy.  
3. **Apply Policy to Firewall Rule:**  
   * Navigate to **Rules and policies** \> **Firewall rules**.  
   * Find your primary **LAN to WAN** rule (the rule that allows internet access for your client VMs).  
   * Click the Edit icon for that rule.  
   * In the rule configuration, scroll down to **Web Policy** (under the Advanced section or similar).  
   * Select the policy you created/modified (e.g., Block\_News\_Policy).  
   * Click **Save** to apply the changes.

### **Testing Procedure**

Use any client VM (Ubuntu or Windows Server) to test web access.

1. **Attempt Access (Client VM):** Open a web browser (e.g., Firefox, Edge) in the Ubuntu or Windows Server VM.  
2. **Test Blocked Category:** Attempt to browse to several major online news websites (e.g., CNN, BBC, Reuters).  
3. **Expected Outcome:** The browser should display a Sophos Firewall block page (e.g., "Web Page Blocked \- Category Blocked") instead of the actual news site.  
4. **Test Allowed Category:** Browse to a general search engine (Google) or a non-news site (e.g., an IT documentation page) to verify that allowed traffic still passes through.  
5. **Monitor Sophos Logs:** Check the **Log Viewer**, filtered by **Web Filter**, to confirm that the block events are correctly logged with the reason "Category Blocked: News and Media."

## **3\. Application Control Security Policy (Block File Sharing Sites)**

This involves creating an **Application Filter Policy** to block application signatures related to file sharing and then linking it to the LAN-to-WAN rule.

### **Configuration Steps**

1. **Create Application Filter Policy:**  
   * Navigate to **Protect** \> **Applications** \> **Application filter**.  
   * Click **Add**.  
   * **Name:** Block\_File\_Sharing  
   * **Template:** Select Allow All.  
   * Click **Save**.  
2. **Add Block Rules to the Policy:**  
   * Select the newly created Block\_File\_Sharing policy and click **Add**.  
   * **Rule Name:** Block File Transfer Apps  
   * **Activities:**  
     * Click **Show All** and select **Application Category**.  
     * Search for and select: **File Transfer** and **P2P** (Peer-to-Peer).  
     * (Optional but Recommended): Also search for and select related categories like **Storage and backup** or specific high-risk applications like **BitTorrent** or **uTorrent**.  
   * **Action:** Set the action to **Deny**.  
   * **Save** the rule and then **Save** the application filter policy.  
3. **Apply Policy to Firewall Rule:**  
   * Navigate to **Rules and policies** \> **Firewall rules**.  
   * Find and edit your primary **LAN to WAN** rule.  
   * Scroll down to the **Identify and control applications (App control)** section.  
   * Select the Block\_File\_Sharing policy you just created.  
   * Click **Save**.

### **Testing Procedure**

You need to attempt to use file-sharing protocols and browser-based storage sites from the client VMs.

1. **Test Browser-Based File Storage (Windows Server/Ubuntu):**  
   * Attempt to access a popular cloud storage/file-sharing site in the browser (e.g., Dropbox, Google Drive, or WeTransfer).  
   * If the site is correctly categorized and the firewall is using DPI (Deep Packet Inspection), access should be blocked, or file uploads/downloads should fail.  
2. **Test P2P/File Transfer Applications (Kali/Parrot/Ubuntu):**  
   * From Kali or Ubuntu, attempt to run a P2P client (like Transmission or qBittorrent) and connect to a known swarm.  
   * Attempt an FTP connection (ftp \<WAN IP\>) if you have an FTP server configured, or attempt to use tools that Sophos detects as file transfer (e.g., scp to an external host if the signature is broad).  
3. **Monitor Sophos Logs:**  
   * Check the **Log Viewer**, filtered by **Application Filter**.  
   * You should see entries corresponding to the denied traffic, showing the blocked application (e.g., BitTorrent, File Transfer) and the source IP (your client VM).