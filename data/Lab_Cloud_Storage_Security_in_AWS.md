# **CYS 523—WEEK 1—LAB**

## **CLOUD SECURITY: CLOUD STORAGE SECURITY IN AWS**

## **Overview**

In this lab, you will learn how to set up and secure an Amazon S3 bucket. We will cover implementing access controls, configuring encryption, and applying bucket policies to protect stored data.

## **Prerequisites**

* An active AWS account (Free Tier is sufficient).  
* Access to the AWS Management Console.

## **Part 1: Creating and Configuring the S3 Bucket**

### **1\. Accessing S3**

1. Log in to the **AWS Management Console**.  
2. Search for **S3** in the services search bar and select it.

### **2\. Creating the Bucket**

1. Click on **Create bucket**.  
2. **Bucket Name:** Enter a unique, lowercase name (e.g., old-school-bucket-111). Bucket names must be globally unique.  
3. **Object Ownership:**  
   * Enable **ACLs (Access Control Lists)** if you need to control ownership of objects uploaded by different accounts.  
   * **Bucket owner preferred:** Ensures the bucket owner has full control over new objects.  
   * **Object writer:** Allows the entity uploading the object to maintain ownership/control.  
   * *Concept:* Think of the bucket as storage (like a fruit basket) and data as objects (the fruit). This setting determines who "owns" the fruit once it's in the basket.  
4. **Block Public Access:** Ensure **Block all public access** is checked. This is a critical security best practice to prevent accidental data exposure.

### **3\. Advanced Settings**

1. **Bucket Versioning:** Enable this to maintain a history of object changes.  
   * *Benefits:* Protects against accidental deletions and aids in data recovery by allowing you to retrieve previous versions of a file.  
2. **Tags:** Add tags for organization (e.g., Key: Department, Value: Learning-and-Academics).  
3. **Default Encryption:** Enable **Server-side encryption** with **Amazon S3 managed keys (SSE-S3)** to ensure data is encrypted at rest.  
4. Click **Create bucket**.

## **Part 2: Implementing Access Policies**

### **1\. Understanding JSON Policies**

AWS uses JSON (JavaScript Object Notation) for identity and access policies.

* **Version:** Specifies the policy language version (use 2012-10-17).  
* **Statement:** An array containing the individual rules.  
* **Effect:** Either Allow or Deny.  
* **Principal:** The user or entity the policy applies to (e.g., \* for everyone).  
* **Action:** The specific AWS operations (e.g., s3:\* for all S3 actions).  
* **Resource:** The ARN (Amazon Resource Name) of the bucket and its objects.

### **2\. Configuring the Bucket Policy**

1. Click on your newly created bucket.  
2. Go to the **Permissions** tab.  
3. Scroll down to **Bucket policy** and click **Edit**.  
4. Enter a policy to restrict access to secure transport (HTTPS) only:

   {

     "Version": "2012-10-17",

     "Statement": \[

       {

         "Effect": "Deny",

         "Principal": "\*",

         "Action": "s3:\*",

         "Resource": \[

           "arn:aws:s3:::old-school-bucket-111",

           "arn:aws:s3:::old-school-bucket-111/\*"

         \],

         "Condition": {

           "Bool": {

             "aws:SecureTransport": "false"

           }

         }

       }

     \]

   }

5. Click **Save changes**.

## **Part 3: Testing Security and Uploading Data**

### **1\. Uploading Objects**

1. Go to the **Objects** tab of your bucket.  
2. Click **Upload** and then **Add files**.  
3. Select files from your local machine (e.g., a PDF or PPT file).  
4. Click **Upload**.

### **2\. Verifying Access Restriction**

1. Click on one of the uploaded files to view its properties.  
2. Find the **Object URL**.  
3. Copy the URL and attempt to open it in a private/incognito browser window.  
4. **Expected Result:** You should receive an **Access Denied** error. This confirms that your public access blocks and bucket policies are correctly preventing unauthorized access over the internet.

## **Summary**

You have successfully:

* Created an S3 bucket with managed encryption and versioning.  
* Configured Object Ownership via ACLs.  
* Implemented a JSON bucket policy to enforce secure transport.  
* Verified that objects remain private and protected from unauthorized public access.