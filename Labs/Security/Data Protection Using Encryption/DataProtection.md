**Data Protection Using Encryption**
 
Cryptography is the conversion of communicated information into secret code that keeps the information confidential and private. Functions include authentication, data integrity, and nonrepudiation. The central function of cryptography is encryption, which transforms data into an unreadable form.

Encryption ensures privacy by keeping the information hidden from people who the information is not intended for. Decryption, the opposite of encryption, transforms encrypted data back into data; it won’t make any sense until it has been properly decrypted.

**Lab Overview**

In this lab, I connected to a file server that is hosted on an Amazon Elastic Compute Cloud (Amazon EC2) instance. I configured the AWS Encryption command line interface (CLI) on the instance. Then I created an encryption key by using the AWS Key Management Service (AWS KMS) to be used to encrypt and decrypt data. Next, I created multiple text files that are unencrypted by default. I used the AWS KMS key to encrypt the files and view them while they are encrypted. Then I decrypted the same files and viewed the contents.

 

**Objectives**

--Create an AWS KMS encryption key

--Install the AWS Encryption CLI

--Encrypt plaintext

--Decrypt ciphertext


**Lab environment**

The lab environment has one preconfigured EC2 instance named File Server. An AWS Identity and Access Management (IAM) role is attached, which allows to connect to the instance by using the AWS Systems Manager Session Manager.

All backend components, such as EC2 instances, IAM roles, and some AWS services, have been built into the lab already.

**Task 1: Create an AWS KMS key**

In this task, I created an AWS KMS key that will be used later in the lab to encrypt and decrypt data.
With AWS KMS, we can create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications. 
**Steps followed:**

**Step 1: Open AWS KMS**
1. In the AWS Console, enter **KMS** in the search bar.
2. Select **Key Management Service**.

**Step 2: Create a Key**
1. Choose **Create a key**.
2. For **Key type**, select:
   - **Symmetric**
3. Click **Next**.

> **Note:**  
> Symmetric encryption uses the same key to encrypt and decrypt data, making it fast and efficient.  
> Asymmetric encryption uses a public key to encrypt data and a private key to decrypt it.



**Step 3: Add Labels**
Configure the following:

- **Alias:** `MyKMSKey`  
- **Description:** `Key used to encrypt and decrypt data files.`  

Click **Next**.



**Step 4: Define Key Administrative Permissions**
1. In the **Key administrators** section:
   - Search for `voclabs`
   - Select the checkbox
2. Click **Next**



**Step 5: Define Key Usage Permissions**
1. In the **This account** section:
   - Search for `voclabs`
   - Select the checkbox
2. Click **Next**



**Step 6: Review and Create**
1. Review all settings.
2. Click **Finish**.



**Step 7: Copy the Key ARN**
1. Select the key **MyKMSKey** from the list.
2. Copy the **ARN (Amazon Resource Name)**.
3. Save it in a text editor.

> ⚠️ You will need this ARN later for encryption and decryption commands.
# AWS Key creation Guide

## Step 1: Open AWS KMS
1. In the AWS Console, enter **KMS** in the search bar.
2. Select **Key Management Service**.

## Step 2: Create a Key
1. Choose **Create a key**.
2. For **Key type**, select:
   - **Symmetric**
3. Click **Next**.

> **Note:**  
> Symmetric encryption uses the same key to encrypt and decrypt data, making it fast and efficient.  
> Asymmetric encryption uses a public key to encrypt data and a private key to decrypt it.



## Step 3: Add Labels
Configure the following:

- **Alias:** `MyKMSKey`  
- **Description:** `Key used to encrypt and decrypt data files.`  

Click **Next**.



## Step 4: Define Key Administrative Permissions
1. In the **Key administrators** section:
   - Search for `voclabs`
   - Select the checkbox
2. Click **Next**



## Step 5: Define Key Usage Permissions
1. In the **This account** section:
   - Search for `voclabs`
   - Select the checkbox
2. Click **Next**



## Step 6: Review and Create
1. Review all settings.
2. Click **Finish**.



## Step 7: Copy the Key ARN
1. Select the key **MyKMSKey** from the list.
2. Copy the **ARN (Amazon Resource Name)**.
3. Save it in a text editor.

> ⚠️ You will need this ARN later for encryption and decryption commands.
