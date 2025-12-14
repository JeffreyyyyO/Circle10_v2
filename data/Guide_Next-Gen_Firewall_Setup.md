# **Guide: Next-Gen Firewall Setup**

# 

# **Sophos Firewall**

## **Installation, Configuration & Setup**

## **Introduction**

This guide provides step-by-step instructions for setting up and configuring the **Sophos Virtual Firewall** from within **VMware Workstation Pro** and the host machine via the web console. Security policies are then configured and tested.

1. ## **Virtualizer** **Download** (from October 2025\)

   1. Download virtualizer (**VMware Workstation Pro 17.5**) for the host machine (**Windows 10 x64**).

2. ## **Virtualizer Installation on Windows 10 Host Machine** (from October 2025\)

   1. Launch executable (**EXE** format) file.  
   2. Read and accept the software **License Agreement.**  
   3. Configure Installation on Setup Wizard  
      1. **“Enhanced Keyboard Driver”** is **not selected** because it supports atypical (international) keyboards and keyboards with extra keys.   
      2. **“Add VMware Workstation console tools into system PATH”** is **selected** because it integrates the console tools into your system, allowing you to run them from any command prompt without specifying the full path to the executable.  
   4. Configure User Experience Settings  
      1. **“Check for product updates on startup”** is **selected** for security reasons. Immediate implementation of patches and updates is good security practice.  
      2. **“Join the VMware Customer Experience Improvement Program”** is **not selected** because it is not desired.  
   5. Configure Shortcuts  
      1. Shortcuts are created in the Start menu but not on the Desktop, for clean organization of the Computer interface.  
   6. Install **VMware Workstation Pro 17.5.**  
      1. After the main configurations are done, click the "Install” button.  
   7. Select the **“License”** button on the “Completed” dialog, to input Software License.  
   8. Enter the license key in the “Enter License Key” dialog and click the “Enter” button.  
   9. Finish Installation by clicking the “Finish” button on the final dialog.

3. ## **Sophos Firewall Download & Pre-Installation**

   1. **Firewall Installers**  
      1. Launch a browser (**Firefox**).  
      2. Visit the Sophos **Firewall Installers** webpage using the link: **https://www.sophos.com/en-us/support/downloads/firewall-installers**  
      3. Scroll down and locate the card titled **“Virtual Installers: Firewall OS for VMware”** and click the contained **Download** button. *This opens the **End User Terms of Use & Export Compliance** webpage in a new tab.*  
   2. **End User Terms of Use & Export Compliance**  
      1. On the just opened **End User Terms of Use & Export Compliance** webpage, fill out the form with the details First Name, Last Name, Company, Email address in the relevant labelled text boxes.  
      2. Check the box at the bottom of the page to both accept the **Sophos End User Terms of Use** and acknowledge the **Sophos Privacy Notice**.  
      3. Click the **Submit Query** button. *This opens the **Sophos File Download** webpage.*  
   3. **Sophos File Download**  
      1. On the just opened **Sophos File Download** webpage, click the download link under “Success\! You have been authorized” to download the **ZIP** file.  
      2. The file download **(VI-21.5.0\_GA.VMW-171.zip)** begins and progresses.  
      3. The file download completes and saves to the **Download** folder.  
   4. **Pre-Installation Setup**  
      1. Launch the **ZIP** file, **VI-21.5.0\_GA.VMW-171.zip,** using a file decompressor/extractor software **(WinRAR)**.  
      2. Extract the contents of the **ZIP** file to a desired location (**SophosFirewall** folder).

4. ## **VMware Network Configuration**

   1. **Launch Virtual Network Editor**  
      1. Launch **VMware Workstation Pro 17.5**.  
      2. Click **Edit** and select **Virtual Network Editor**. *This opens the **Virtual Network Editor** window*.  
   2. **Configure Virtual Network**  
      1. Click **Change Settings** at the bottom right of the window to secure Administrator permissions, in order to modify the network configuration.  
      2. In the network adapter list at the top, select **VMnet1.**  
      3. Go under **VMnet Information**  
      4. Select **Host-only (connect VMs internally in a private network),** setting **Type** to **Host-only**.  
      5. Uncheck the **Use local DHCP service to distribute IP addresses to VMs** box.  
      6. In the **Subnet IP** text box, input 192.168.56.0   
      7. In the **Subnet mask** text box, input 255.255.255.0   
      8. Click **Apply** then click **OK.**

5. ## **Firewall Machine Creation & Network Configuration**

   1. **Import Firewall Virtual Machine.**  
      1. Click **File** then click **Open.**  
      2. Locate extracted contents of download Sophos Firewall **ZIP** file (inside the **SophosFirewall** folder).  
      3. Select the **sf\_virtual.ovf** file and click **Open**. *This opens the **Import Virtual Machine** window.*  
      4. In the **Import Virtual Machine** window:  
         * Fill the **Name** text box with the virtual machine name: **Sophos-Firewall**  
         * Fill the **Storage path** text box with the intended storage address of the virtual machine.   
         * Click **Import.**  
      5. After the **Importing Sophos-Firewall** progress bar completes, a new virtual machine called **Sophos-Firewall** is added to the Library panel on the **VMware Workstation Pro 17.5** main interface.  
   2. **Configure Firewall Virtual Machine Network Adapters**  
      1. In the Library panel of the **VMware Workstation Pro 17.5** main interface, right-click the **Sophos-Firewall** virtual machine and select **Settings…** *This opens the **Virtual Machine Settings** window.*  
      2. Within the **Hardware** tab, inside the navigation pane, select the device, **Network Adapter.**  
      3. In the corresponding **Network Connection** section on the right, select the **Custom: Specific virtual network** radio-button.  
      4. In the drop down menu beneath, select **VMnet1 (Host-only).**  
      5. Ensure that in the navigation pane, the device, **Network Adapter 2**, has the default summary, **Bridged (Automatic).**  
      6. Click the **OK** button.

6. ## **Firewall Operating System Setup**

   1. **Launch and Install the Firewall Operating System**  
      1. In the Library panel, located on the left side of the **VMware Workstation Pro** main interface, select **Sophos-Firewall.**  
      2. In the **Sophos-Firewall** tab, select **Power on this virtual machine.**  
      3. On the opening bootloader screen, titled **GNU GRUB version 2.02**, select **\*21\_5\_0\_171** and press **Enter.**  
      4. On the next screen titled **Booting ‘21\_5\_0\_171’** type the password **admin** and press **Enter.**  
      5. The next screen is the **Main Menu** screen. Press **1** to select **Network Configuration**. Press **Enter.**  
      6. The next screen is the **Network Configuration Menu** screen. Press **1** to select **Interface Configuration**. Press **Enter.**  
      7. The next screen is the **Network Settings** screen. Press **Enter** to continue.  
      8. On the next screen with the dialog **Set IPv4 Address (y/n) : No (Enter) \>** type **Y**.  
      9. The next screen is the **Network Configuration of Ethernet PortA**.  
         * For **New IP Address** type 192.168.56.201. Press **Enter**.  
         * For **New Netmask** type 255.255.255.0. Press **Enter**.  
      10. The next screen is the **Network Configuration of Ethernet PortB**. Press **Enter**.  
      11. On the next screen with the dialog **Set IPv6 Address (y/n) : No (Enter) \>** type **N**.  
      12. The next screen is the **Network configuration Menu** screen. Press **0** to select **Exit**. Press **Enter.**  
      13. The next screen is the **Main Menu** screen.

7. ## **Firewall Web Admin Console Setup**

   1. **Setup the Firewall in the Web Admin Console**  
      1. Minimize VMware and keep it running.  
      2. Launch a browser **(Firefox)** window.  
      3. Go to the IP address that was set up for PortA on port 4444  
         * In the address bar, type in the address: **https://192.168.56.201:4444**  
         * Press **Enter**.  
      4. A notice page with the text **“Warning: Potential Security Risk Ahead”** appears because the port is not recognized by the browser as secure.  
         * Click the **Advanced…** button.  
         * Click the **Accept the Risk and Continue** button.  
      5. The **Sophos Firewall** initial setup frontpage appears. Click **Start Setup**.  
      6. On the **Basic configuration** webpage:  
         * Create a new administrator account password by typing it and re-typing it in the **Default administrator’s new password** text box and the **Reenter the password** text box, respectively.  
         * Click the **Continue** button**.** *This opens the **Secure storage master key** webpage.*  
      7. On the **Secure storage master key** webpage:  
         * Create a new administrator account password by typing it and re-typing it in the **Create secure storage master key** text box and the **Repeat the master key** text box, respectively.  
         * Check the **I have stored the master key in a password manager or another secure location** checkbox.  
         * Click the **Continue** button**.** *This opens the **Name and time zone** webpage.*

      8. On the **Name and time zone** webpage:  
         * In the **Firewall name** text box, type a name for the firewall.  
         * Select the appropriate time zone in the dropdown menu under the map.  
         * Confirm that the time and date in the **Current time** section on the web interface matches the current time on the wider operating system.  
         * Click the **Continue** button. *This opens the **Register your firewall** webpage.*  
      9. On the **Register your firewall** webpage:  
         * Click the **I have an existing serial number** button.  
         * Since there is no serial number, it is time to register for one.  
   2. **Acquire serial number for free trial access.**  
      1. While keeping the current one still open, open a new browser tab.  
      2. Visit the **Sophos Firewall free trial** webpage, using the following URL: [**https://www.sophos.com/en-us/products/next-gen-firewall/free-trial**](https://www.sophos.com/en-us/products/next-gen-firewall/free-trial)  
         * Fill and submit the registration form on the webpage.  
           * Enter required details (First Name, Last Name, Email, Company, Number of Employees, Job role, Phone number, Country, and State).  
           * Clear CAPTCHA.  
           * Click the **Submit** button.  
      3. A **Thank You** confirmation page opens, instructing you to check your email inbox for the **evaluation serial number**.  
      4. Locate the serial number in your email inbox and copy it.  
   3. **Resume Web Admin Console Setup**  
      1. Close the **Thank You** browser tab and return to the **Register your firewall** webpage.  
      2. On the **Register your firewall** webpage:  
         * Paste the copied serial number into the **I have an existing serial number** text box.  
         * Click the **Continue** button. *This leads to an error as the serial number registration process fails.*  
         * Check the **I do not want to register now** checkbox.  
         * Click the **Continue** button. *This opens the **Basic setup is complete** webpage.*  
      3. On the **Basic setup is complete** webpage:  
         * Review the contents to see the details of the Sophos Firewall subscriptions  
         * Click the **Continue** button. *This opens the **Network configuration (LAN)** webpage.*  
      4. On the **Network configuration (LAN)** webpage:  
         * Confirm the IP address in the **LAN IP address** text box is the same as the previously configured PortA IP address.  
         * Uncheck the **Enable DHCP** checkbox.  
         * Click the **Continue** button. *This opens the **Network protection** webpage.*  
      5. On the **Network protection** webpage:  
         * Check all four checkboxes titled:  
           * Protect users from network threats;  
           * Protect users from the suspicious and malicious websites;  
           * Scan files that were downloaded from the web for malware; and  
           * Send suspicious files to zero-day protection  
         * Click the **Continue** button. *This opens the **Notifications and backups** webpage.*  
      6. On the **Notifications and backups** webpage:  
         * In the **Recipient’s email address** and **Sender’s email address** text boxes, input the two email addresses  that belong to you, for receiving notifications.  
         * In the **Encryption password** and **Confirm encryption password**  text boxes, input your encryption password which will be used to encrypt files and other network resources.  
         * Leave the **Use external mail server** checkbox unchecked.  
         * Click the **Continue** button. *This opens the **Configuration summary** webpage.*  
      7. On the **Configuration summary** webpage:  
         * Review all configuration details in the text area for a final approval.  
         * After approval, click the **Finish** button. *This opens the **Finishing** webpage.*  
      8. On the **Finishing** webpage:  
         * The loading spinner next to **Applying configuration changes,** spins until the action is complete. The spinner then changes to a checked circle.  
         * The loading spinner next to **Restart** spins until the Sophos Firewall VM completes the restart. The spinner then changes to a checked circle.  
         * A **Sophos Firewall** sign-in page pops up.

8. ## **Firewall Console Access**

   1. **Login to the Firewall Console**  
      1. On the **Sophos Firewall** sign-in page:  
      2. In the **Username** text box, type in the username “**admin”** from **Step 6a.**  
      3. In the **Password** text box, type in the password which was set up in **Step 7a**.  
      4. Click the **Login** button. *This opens the **Register your firewall** webpage*  
      5. On the **Register your firewall** webpage:  
         * The serial number is shown pasted in the **I have an existing serial number** text box.  
         * Check the **I do not want to register now** checkbox.  
         * Click the **Continue** button. *This opens the **Control center** interface.*  
   2. **Control center access**  
      1. The main Firewall interface launches, giving access to the Firewall web console.  
      2. The interface provides access to firewall analytics, controls and configurations.  
      3. Note that despite clicking the **I do not want to register now** checkbox on the **Register your firewall** webpage, the serial number appears on the interface.