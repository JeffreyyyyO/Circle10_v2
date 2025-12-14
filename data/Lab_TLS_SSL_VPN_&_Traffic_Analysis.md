# **Lab: TLS/SSL, VPN & Traffic Analysis**

## **TLS/SSL Configuration, VPN Setup & Traffic Analysis**
This guide outlines the steps for setting up a secure web server, configuring SSL/TLS, and analyzing network traffic using Kali Linux and Wireshark.

## **Prerequisites: Virtualizer Configuration**

Before starting the Kali Linux VM, ensure the following settings are configured in the Virtualizer (VMware, VirtualBox, UTM, IPFire, Pfsense, etc):

1. **Network Settings:**  
   * Set the network adapter to **NAT Network** or **Bridged Adapter** (NAT is used for this demo).  
   * Ensure the VM has internet access.  
2. **Clipboard Sharing (Optional but Recommended):**  
   * Go to **Settings \> General \> Advanced**.  
   * Set **Shared Clipboard** and **Drag'n'Drop** to **Bidirectional**. This allows copying commands from your host to the VM.  
3. **System Resources:**  
   * **RAM:** Recommended 4GB (4096 MB).  
   * **Processors:** Recommended 2 CPUs.

## **Phase 1: Kali Linux Setup & Basic Checks**

### **1\. Install Guest Additions (Optional)**

`Improves display resolution and mouse integration.`  
`sudo apt update`  
`sudo apt install virtualbox-guest-x11 -y`  
`sudo reboot`

### **2\. Verify Network Connection**

Once the VM reboots, open the terminal and check your IP address to ensure network connectivity.  
`ip a`  
`# OR`  
`ifconfig`

*Note: Identify your network interface (usually eth0) and IP address.*

## **Phase 2: Apache Web Server Setup**

### **1\. Install Apache HTTP Server**

`sudo apt update`  
`sudo apt install apache2 -y`

### **2\. Check and Start Apache Service**

Check if the service is running. It is often disabled by default.  
`systemctl status apache2`

If it is disabled or inactive, start it:  
`sudo systemctl start apache2`

### **3\. Verify Installation**

Open the web browser in Kali (Firefox) and type your IP address or localhost in the address bar. You should see the "Apache2 Debian Default Page."

### **4\. Host a Custom Web Page (Login Form)**

Navigate to the web root directory and create a custom HTML file to simulate a login page.  
**Navigate to Web Root:**  
`cd /var/www/html`

**Create a New File:**  
`sudo touch altschool_login.html`  
`sudo nano altschool_login.html`

**Add HTML Content:**  
Paste a simple HTML login form (e.g., from W3Schools) into the file.  
Example Structure:  
`<!DOCTYPE html>`  
`<html>`  
`<body>`  
`<h2>Login Form</h2>`  
`<form action="/action_page.php">`  
  `<label for="fname">Username:</label><br>`  
  `<input type="text" id="fname" name="fname"><br>`  
  `<label for="lname">Password:</label><br>`  
  `<input type="text" id="lname" name="lname"><br><br>`  
  `<input type="submit" value="Submit">`  
`</form>`   
`</body>`  
`</html>`

Save and exit (Ctrl+X, Y, Enter).  
Access the Page:  
In your browser, go to: http://localhost/altschool\_login.html (or use your IP).  
**Note:** The connection is currently Not Secure.

## **Phase 3: Traffic Analysis (Unsecured HTTP)**

### **1\. Configure Wireshark**

1. Launch **Wireshark**.  
2. Select your active network interface (e.g., eth0 or any).  
3. Click the blue shark fin icon to **Start Capturing Packets**.

### **2\. Capture Credentials**

1. Go back to your browser and the altschool\_login.html page.  
2. Enter dummy credentials (e.g., Username: Victor, Password: password) and click **Submit**.  
3. You may see a "404 Not Found" error because the action page doesn't exist. This is fine; the data was still sent.

### **3\. Analyze the Traffic**

1. Stop the Wireshark capture.  
2. Filter for **HTTP** traffic or look for the POST or GET request.  
3. Right-click the packet associated with the login action.  
4. Select **Follow \> HTTP Stream**.  
5. **Result:** You will see the username and password in **plain text**. This confirms that HTTP sends data unencrypted.

## **Phase 4: SSL/TLS Configuration (HTTPS)**

To secure the data, we will generate a Self-Signed SSL Certificate.

### **1\. Generate the Certificate**

Run the following command to generate a certificate valid for 365 days.  
`sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt`

**Fill in the Prompted Details:**

* **Country Name (2 letter code):** e.g., NG  
* **State/Province:** e.g., Lagos  
* **Locality:** e.g., Lagos  
* **Organization Name:** e.g., AltSchool Africa  
* **Common Name:** AltSchool Server or your IP/localhost.  
* **Email:** e.g., victor@altschoolafrica.com

### **2\. Configure Apache to Use the Certificate**

Edit the default SSL configuration file.  
`sudo nano /etc/apache2/sites-available/default-ssl.conf`

Find the following lines and update the paths to match the keys you just generated:  
`SSLCertificateFile      /etc/ssl/certs/apache-selfsigned.crt`  
`SSLCertificateKeyFile   /etc/ssl/private/apache-selfsigned.key`

*Note: Ensure the paths match exactly where you saved the files in step 1\.*  
Save and exit (Ctrl+X, Y, Enter).

### **3\. Enable SSL Module and Site**

Enable the SSL module in Apache and activate the SSL virtual host.  
`sudo a2enmod ssl`  
`sudo a2ensite default-ssl`

### **4\. Validate and Restart Apache**

Check for syntax errors:  
`sudo apache2ctl configtest`

*Note: A warning about "Could not reliably determine the server's fully qualified domain name" is normal for localhost.*  
Restart Apache to apply changes:  
`sudo systemctl restart apache2`

## **Phase 5: Traffic Analysis (Secured HTTPS)**

### **1\. Verify HTTPS Access**

1. Go to your browser and access your page using https:  
   https://localhost/altschool\_login.html  
2. **Warning Page:** You will see a "Potential Security Risk" or "Connection not secure" warning. This is expected because we used a **Self-Signed Certificate** (not trusted by browsers like Chrome/Firefox).  
3. Click **Advanced** \-\> **Accept the Risk and Continue**.  
4. You should now see your login form with a padlock icon (possibly with a warning triangle, but the traffic is encrypted).

### **2\. Capture Encrypted Traffic**

1. Start a new capture in **Wireshark**.  
2. Enter credentials in your login form and submit.  
3. Stop the Wireshark capture.  
4. Filter for **TCP** or **TLS** traffic.  
5. Locate the packets interacting with the server (TLSv1.2 or TLSv1.3).  
6. Right-click and select **Follow \> TCP Stream** or **TLS Stream**.

### **3\. Result**

Unlike the HTTP test, the content in the stream will be **unreadable (garbled text)**. You cannot see the username or password. This confirms the SSL/TLS encryption is working.

## **Phase 6: Encrypted VPN Traffic Analysis (Proton VPN & Wireshark)**

This phase provides hands-on experience in capturing and analyzing encrypted traffic, specifically focusing on a VPN connection using Proton VPN and WireShark.

### **Prerequisites:**

* Proton VPN installed and configured (Free account works).  
* Wireshark installed on your system.  
* Basic understanding of networking protocols (ICMP, TCP/TLS).

### **1\. Proton VPN Setup and Connection**

1. **Download and Install Proton VPN:** Open a browser and search for "Proton VPN". Create a free account and download the installer for your operating system (Windows, Linux, or macOS).  
2. **Sign In:** Launch the installed Proton VPN application and sign in using your credentials.  
3. **Connect:** Click the **Connect** button. The application will connect you to the fastest available free server (e.g., in the Netherlands). Note the VPN's assigned IP address.

### **2\. Verify VPN Interface and Connectivity**

1. **Check Network Interfaces:** Open your Command Prompt (Windows) or Terminal (Linux/Mac).  
   * **Windows:** Run `ipconfig`  
   * **Linux/Mac:** Run `ifconfig` (or `ip a`)  
   * **Verification:** Confirm that a new network adapter/interface (e.g., "Proton VPN") is now listed with an IP address, indicating the VPN tunnel is active.  
2. **Test Connectivity (Ping):** Ping the VPN's assigned IP address (e.g., `ping 194.0.59.60.69`). A successful response confirms active communication through the VPN tunnel.  
   * **Command (Example):** `ping 194.0.59.60.69`

### **3\. Capture VPN Traffic in Wireshark**

1. **Launch Wireshark.**  
2. **Select VPN Interface:** Identify and select the active **Proton VPN** interface/adapter listed in Wireshark's capture list.  
3. **Start Capture:** Click the shark fin icon to begin capturing packets.  
4. **Generate Traffic (Client Request):** While Wireshark is capturing, use a client command to initiate a secure connection:  
   * Open your terminal and run a CURL command to a secure website:  
     * **Command (Example):** `curl -v https://altschoolafrica.com`  
     * *Note: The `-v` flag provides verbose output, showing the TLS/SSL negotiation process.*  
5. **Stop Capture:** Stop the Wireshark capture once the CURL command completes and packets have been recorded.

### **4\. Analyze Encrypted and Control Traffic**

1. **Filter ICMP Protocol (Ping Test):**  
   * In the Wireshark filter bar, type `icmp` and press Enter.  
   * **Result:** You will see the Echo Request and Echo Reply packets generated by the Ping test (Step 2.2), confirming ICMP communication.  
2. **Filter TLS Protocol (Secure Traffic):**  
   * In the Wireshark filter bar, type `tls` and press Enter.  
   * **Result:** You will see the encrypted traffic and the TLS handshake sequence (Client Hello, Server Hello, Change Cipher Spec, etc.) related to the CURL request.  
3. **Inspect Handshake:** Locate a packet identified as **Client Hello**.  
   * Expand the **Transport Layer Security** section in the packet details pane.  
   * **Analysis:** You can identify the TLS version (e.g., TLS 1.3), the Cipher Suites offered by the client, and the extensions used for establishing the secure connection.  
4. **Inspect Encrypted Data:** Attempt to follow a stream within the TLS traffic.  
   * Right-click a TLS packet (e.g., "Application Data").  
   * Select **Follow \> TCP Stream**.  
   * **Result:** The stream content will appear as unreadable, garbled binary data. This confirms that even while using the VPN interface, the secure TLS connection within the tunnel remains encrypted and unreadable to Wireshark.  
5. **Conclusion:** The VPN successfully tunnels and encrypts the traffic, and the nested TLS protocol further secures the application layer data, rendering the content unreadable to passive sniffers like Wireshark.

### **Note:**

This guide was generated from the LMS labs.
