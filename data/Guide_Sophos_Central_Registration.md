# **SOPHOS CENTRAL REGISTRATION**

# Completing the Firewall Registration & FINALLY Accessing the Sophos Central Dashboard

# **Introduction**

This guide contains the steps required to complete the Sophos Virtual Firewall registration and resolve the local firewall to the Sophos Central account.

These extensive steps were not covered by **the original guide** and the gap left by incomplete registration has become a pain point for those of us exploring the Sophos features further, beyond the firewall & security policies.

I believe this solves the greater general registration problem we’re facing with the firewall & helps us access the **Sophos Central Dashboard** in order to reach the **Endpoint Protection \- Installers** page (from the demonstration in **Live Class: 2nd Sem—Month 2—Week 1—B).**

# **Procedure**

1. ## **Access the Free Trial Site**

   1. In your browser, visit the free trial site: [**https://www.sophos.com/en-us/products/sophos-central/free-trial**](https://www.sophos.com/en-us/products/sophos-central/free-trial)  
   2. If you’ve already been to this page to request a verification code, you might be greeted with a **“Welcome back, \[*your\_firstname*\]\!”** message.  
   3. Click **Edit your info** (next to the welcome message). *This leads to the **Registration info** page.*
   4. **If you haven’t been here before**, you’ll see this page below. Click the **Sign Up** button. *This also leads to the **Registration info** page.*
   5. You might be apprehensive because you’ve been here before. Just follow through.

2. ## **Confirm Registration Information**

   1. On the **Registration info** page, you will find some text boxes for registration information.  
   2. If you’ve been here before/already have an account, you will see your data already filled in.  
   3. **If you haven’t been here before**, you’ll find the text boxes blank. Fill in your information.  
   4. Solve the **CAPTCHA**, Check the **“Terms & conditions”** checkbox, and click the **Submit** button. *This opens the **Thank You** page.*

3. ## **Send Activation Email**

   1. On the **Thank You** page, you’re prompted to **“Check your email for your activation link.”**  
   2. Because I didn’t get the email the first time, I clicked **Didn’t get an email?** (which may have prompted it to get sent, this time). *Clicking it opened a support page which didn’t help. You can ignore it.*

4. ## **Email Activation**

   1. Open your email inbox and **WAIT.** You will receive an email. (Mine took 15-20 minutes to send. *It’s unfortunate some of us didn’t receive that email during the first go, before the assessment deadline*).  
   2. Open the email, with the subject **‘Activate your Sophos Central account.’** Click the green coloured **Create Password** button. *This opens a **Sophos Central Admin ‘Activate your account’** page.*

5. ## **Sophos Account Activation**

   1. On the **Activate your account** page, fill in the **CREATE PASSWORD** and **CONFIRM PASSWORD** text boxes with a befitting password, compliant with the criteria listed under the **Password must contain:** column.  
   2. For the **CENTRAL ADMIN PORTAL** drop-down menu, leave on the default entry, **UAE**, or select whichever one you like. I selected **US East** because the **UAE** option had a notice at the bottom saying it didn’t have the **ITDR** (Identity Threat Detection and Response) and Mobile capabilities (I don’t know if I’ll need it, but I wanted the fuller option).  
   3. Click the first two mandatory **End User Terms of Use** and **Privacy Notice** checkboxes and click the third optional checkbox if you like.  
   4. Click **Activate account**. *This opens the **Sophos Central Dashboard.***

6. ## **Sophos Central Dashboard**

   1. Once you reach the **Sophos Central Dashboard,** the account activation process is now complete.
   2. Your Email is now fully registered (and you can now access the **Endpoint Protection \- Installers** page), **BUT** you need to **Claim your Firewall with Sophos Central.**

7. ## **Claim your Firewall with Sophos Central**

   1. In your browser, visit [**https://central.sophos.com/**](https://central.sophos.com/)   
   2. In the **Sign in with your Sophos ID** page, type your email address into the **Email address** text box.  
   3. Click the **Continue** button.  
   4. In the next page, with your email already accepted, don’t bother using the password you already saved. Click **Forgot Password.** This sends a verification email with a code to your email inbox.  
   5. Go to your email inbox and copy the verification code.  
   6. In the next **Reset Your Password** page, follow the prompt and paste the verification code into the **Verification Code** text box.  
   7. Click the **Verify code** button.  
   8. Fill out the new password in the **New Password** and **Confirm New Password** text boxes.  
   9. Click the **Reset Password** button. *This opens a **Check Your Email and Provide Security Code** page and sends **ANOTHER** security code to your email.*  
   10. Go to your email inbox and copy the verification code.  
   11. In the **Check Your Email and Provide Security Code** page, follow the prompt and paste the verification code into the **Verification Code** text box.  
   12. Click the **Continue** button. This opens a **Set up MFA Method** page.  
   13. On the **Set up MFA Method** page, I select my desired MFA method (Passkey) by clicking the **Passkey** button.  
   14. Click the **Set up now** button.  
   15. This opens a Windows dialog box for entering my native computer pin. I enter my **Computer PIN.** *This opens a sign in progress page*.

   **P.S.**   
    The next time you try to log back in, you go to the **Sign in page** and type your email and click **Continue**, Click **Login with your Passkey**, and type your PIN in the Windows Security dialog. *This opens a sign-in progress page*.

8. ## **Return to Sophos Central Dashboard**

   1. On the sign-in progress page, the sign-in process completes and redirects to the **Sophos Central Dashboard** from [**Step 6**](#sophos-central-dashboard)**.**

9. ## **Apply Registration to Firewall from Sophos Central Dashboard**

   1. Open your virtualizer and power on your **Sophos Virtual Firewall**.

   2. Access the web console by visiting the Firewall Port A (LAN) IP address from the browser address bar. *This opens the Sophos Firewall login page.*  
   3. On the Sophos Firewall login page, input the relevant credentials in the **Username** and **Password** text boxes, and click the **Login** button. *This opens the **Register your firewall** page.*

   4. On the **Register your firewall** page, you will find your serial number already input in the **I have an existing serial number** text box. Click the **Continue** button. *This opens the **Claim your firewall with Sophos Central** page.*

   5. On *the **Claim your firewall with Sophos Central** page*, click the **Claim in Sophos Central** button. *This opens the **Sophos Central Dashboard** (as shown in [**Step 6**](#sophos-central-dashboard)).*

   6. On the ***Sophos Central Dashboard*****,** a **Claim firewall** dialog box with a **Serial number** and **Model** text boxes filled in and 2 firewall-claim options to select from, appears. Select the **Claim with 30 days Xstream Protection…** button (because it has more features), then click the **Claim firewall** button. *This opens a ‘**Basic setup is complete’** page and sends a confirmation email with the subject, **Claim Firewall.***

   7. On the **Basic setup is complete** page, you will find a list of relevant subscriptions, their statuses and their expiration dates for your review. Click the **Continue** button at the bottom of the page. *This returns the **Sophos Firewall Web console.***

10. ## **Apply Registration to Firewall from Web Console**

    1. The **Sophos Firewall Web console** opens, with the registered account seemingly merged with the virtual firewall *(find the company name—from the trial registration—present in the upper right corner of the screen)*.  
    2. In the **Navigation Pane**, under **SYSTEM,** click **Sophos Central,** opening the **Sophos Central** page.
    3. On the **Sophos Central** page *(with most options greyed out)*, click the **Register** button, inside the **Sophos Central registration** section. This opens a **Register firewall with Sophos Central** form-window.
    4. On the **Register firewall with Sophos Central** form, click the **Use email address** button and fill the **Email address** and **Password** text boxes with the login credentials used in [**Step 9**](#apply-registration-to-firewall-from-sophos-central-dashboard). Click the **Register** button.  
    5.  The **Sophos Central** page is returned with the formerly greyed out options now activated and a new Registration date shown in the **Sophos Central registration** section.  
    6. A pop-up message appears with the message **“Firewall registered with Sophos Central Successfully.”**

11. ## **REGISTRATION COMPLETE**

The **Sophos Virtual Firewall** registration is now **ABSOLUTELY** complete with the serial number fully applied, while being fully merged with the **Sophos Central** account (and the **Sophos Central Dashboard**).