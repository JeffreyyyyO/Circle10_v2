# **Guide: Sophos Firewall Security Configuration 2**

# **Sophos Firewall (SFOS) Configuration of 2 Security Policies**

Environment: SFOS v21.5.0 on VMware Workstation 17.5  
Management IP: https://192.168.56.201:4444  
This guide details the steps to configure **Application Control** (to block/allow specific applications) and **Access Time Policies** (to schedule when network access is permitted).

## **Prerequisites: Accessing the Web Admin Console**

1. Open a web browser on your Windows 10 host.  
2. Navigate to https://192.168.56.201:4444.  
   * *Note: You may see a certificate warning. Click "Advanced" and "Proceed" (or "Accept Risk") to bypass it.*  
3. Login with your administrator credentials (default is often admin).

## **Part 1: Configure an Application Security Policy**

This section creates a policy to block a specific category of applications (e.g., "Social Networking") and applies it to your LAN traffic.

### **Step 1: Create the Application Filter**

1. In the left-hand navigation menu, go to **Protect** \> **Applications**.  
2. Select the **Application filter** tab.  
3. Click the **Add** button.  
4. **Name**: Enter a descriptive name (e.g., Block\_Social\_Media).  
5. **Template**: Keep "Allow All" selected (this allows us to specifically blacklist items while allowing everything else).  
6. Click **Save**.

### **Step 2: Add Rules to the Filter**

1. You will now see your new policy in the list. Click the **Edit** icon (pencil) next to Block\_Social\_Media.  
2. Click the **Add** button inside the policy editor.  
3. **Category**:  
   * Click the dropdown or search icon.  
   * Select **Social Networking** (or any specific app like *Facebook*).  
   * You can select multiple categories or specific applications using the "Select individual application" option.  
4. **Action**: Set this to **Deny**.  
5. **Schedule**: Leave as All the time (unless you only want to block it during work hours).  
6. Click **Save** to add the rule to the filter.  
7. **Crucial Step**: Ensure your new "Deny" rule is at the **top** of the list (Rule ID 1), and the "Allow All" rule is at the bottom. The firewall processes rules top-down.  
8. Click **Save** at the bottom of the page to finalize the policy.

### **Step 3: Apply the Policy to a Firewall Rule**

1. Go to **Protect** \> **Rules and policies** \> **Firewall rules**.  
2. Locate your standard LAN-to-WAN rule (often named "Default Network Policy" or created manually).  
   * *If you don't have one, create a new rule with Source Zone: LAN, Source Network: Any, Destination Zone: WAN, Destination Network: Any.*  
3. Click the **Edit** icon (pencil) for that rule.  
4. Scroll down to the **Security features** section.  
5. Locate **Web filtering and application control**.  
6. Under **Application control**, select your new policy: Block\_Social\_Media.  
7. Click **Save**.

*Result: Traffic passing through this rule will now be checked against your application filter. Social media traffic will be dropped.*

## **Part 2: Configure an Access Time Policy (Schedule)**

This section creates a time schedule (e.g., "Work Hours Only" or "No Night Access") and applies it to a firewall rule to restrict when the internet can be used.

### **Step 1: Create a Schedule Object**

1. In the left-hand navigation menu, go to **System** \> **Profiles**.  
2. Select the **Schedule** tab.  
3. Click **Add**.  
4. **Name**: Enter a name (e.g., Office\_Hours\_Only).  
5. **Recurring**: Checked.  
6. Set the time blocks:  
   * **Days**: Select Mon, Tue, Wed, Thu, Fri.  
   * **Start Time**: 09:00 (9 AM).  
   * **Stop Time**: 17:00 (5 PM).  
   * *Note: You can click "Add" to define different times for weekends if needed.*  
7. Click **Save**.

### **Step 2: Apply the Schedule to a Firewall Rule**

There are two ways to use this schedule. You can either apply it to the entire Firewall Rule (controlling *when* the rule is active) or inside a Web Policy (controlling *when* surfing is allowed). We will apply it to the **Firewall Rule** for stricter control.

1. Go to **Protect** \> **Rules and policies** \> **Firewall rules**.  
2. Click **Add firewall rule** \> **New firewall rule** (or edit an existing one).  
3. **Rule Name**: LAN\_Access\_Scheduled.  
4. **Source Zone**: LAN.  
5. **Source Networks**: Any (or specific IP/User).  
6. **Destination Zone**: WAN.  
7. **Destination Networks**: Any.  
8. **Action**: Accept.  
9. Scroll down to the **Other** section (or look for the "Rule Schedule" dropdown near the top in newer versions).  
10. **Rule schedule**: Select your custom schedule Office\_Hours\_Only.  
    * *Logic: This rule will ONLY be active during the times defined in the schedule. Outside those hours, the rule effectively "disappears," and traffic will hit the default "Drop All" rule at the bottom.*  
11. Ensure **Log firewall traffic** is checked.  
12. Click **Save**.

### **Verification**

1. **Application Control**: Try to access Facebook. It should be blocked, while other sites (like Google) should load. You can check **Monitor & Analyze** \> **Diagnostics** \> **Log Viewer** \> **Application filter** to see the blocks.  
2. **Time Policy**: If you are outside the configured hours (e.g., it is currently 8 PM and your schedule ends at 5 PM), internet access should completely stop for machines hitting that rule.