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
>
> **Summary of Task 1**
In this task,I created a symmetric AWS KMS key and gave ownership of that key to the voclabs IAM role that was pre-created for this lab.

<br>

**Task 2: Configure the File Server instance**

In this task, I configured AWS credentials to allow access to the KMS key and installed the AWS Encryption CLI (`aws-encryption-cli`) to perform encryption and decryption operations.

**Steps followed:**

**Step 1: Configure AWS Credentials**

To use your AWS KMS key, you must configure AWS credentials on the EC2 instance.

This allows the instance to:
- Authenticate with AWS
- Access AWS Key Management Service (KMS)
- Use the KMS key you created earlier

> ⚠️ Ensure the instance has the correct permissions (e.g., IAM role or configured credentials).



**Step 2: Install AWS Encryption CLI**

Install the AWS Encryption CLI tool, which will be used to encrypt and decrypt data.

<br>

**Summary of Task 2**

In this task, you configured the AWS credentials file, which provides the ability to use the AWS KMS key that you created earlier. You then installed the AWS Encryption CLI, so that you can run encryption commands.


**Task 3: Encrypt and decrypt data**

In this task, I created a text file with mock sensitive data in it. I used encryption to secure the file contents. Then, I decrypted the data and viewed the file contents.

**Steps followed:**

**Step 1: Create text file**
     
     ```
      touch secret1.txt secret2.txt secret3.txt
      echo 'TOP SECRET 1!!!' > secret1.txt
     ```
   

**Step 2: Create a directory to output the encrypted file**
    
    ```mkdir output
    ```
    
**Step 3: Copy and paste the following command to a text editor**
   
   ```
      keyArn=(KMS ARN)
   ```
   In the text editor, I replaced (KMS ARN) with the AWS KMS ARN that I copied in task 1.
   
   Now, when I run this command, it saves the ARN of the AWS KMS key in the $keyArn variable. When we encrypt by using an AWS KMS key, we can identify it by using a key ID, key ARN, alias name, or alias ARN.

**Step 4: To encrypt the secret1.txt file, I used the following command**  

   ```
     aws-encryption-cli --encrypt \
                     --input secret1.txt \
                     --wrapping-keys key=$keyArn \
                     --metadata-output ~/metadata \
                     --encryption-context purpose=test \
                     --commitment-policy require-encrypt-require-decrypt \
                     --output ~/output/
```

The following information describes what this command does:

The first line encrypts the file contents. The command uses the **--encrypt** parameter to specify the operation and the **--input** parameter to indicate the file to encrypt.

The **--wrapping-keys** parameter, and its required key attribute, tell the command to use the AWS KMS key that is represented by the key ARN.

The **--metadata-output** parameter is used to specify a text file for the metadata about the encryption operation.

As a best practice, the command uses the **--encryption-context** parameter to specify an encryption context.
The **–commitment-policy** parameter is used to specify that the key commitment security feature should be used to encrypt and decrypt.

The value of the **--output parameter**, ~/output/, tells the command to write the output file to the output directory.

**Step 5: To determine whether the command succeeded, I used the following command:**

```
  echo $?
```

The command succeeded, the value of $? returned as  0. If the command had failed, the value would have been nonzero.

**Step 6: To view the contents of the newly encrypted file, I used the following command:**

```
   cd output
   cat secret1.txt.encrypted
```

[Click for Encryption Algorithm Diagram](https://github.com/gmonika-blip/my-aws-restart-journey/blob/9746ad93ad7f282974de25ce855fe68c28baab4c/Labs/Security/Data%20Protection%20Using%20Encryption/EncryptionAlgorithm-Lab278.png)


The encryption and decryption process takes data in plaintext, which is readable and understandable, and manipulates its form to create ciphertext, which is what you can see in this [Screenshot](https://github.com/gmonika-blip/my-aws-restart-journey/blob/5a325815ba5bc50537b1907246978d629a35a1af/Labs/Security/Data%20Protection%20Using%20Encryption/EncrytedFile-Lab278.png)  that I took after running this command.

When data has been transformed into ciphertext, the plaintext becomes inaccessible until it's decrypted.

**Step 7: Decrypt the secret1.txt.encrypted file**

To decrypt the file, I executed the following command:

```
    aws-encryption-cli --decrypt \
                     --input secret1.txt.encrypted \
                     --wrapping-keys key=$keyArn \
                     --commitment-policy require-encrypt-require-decrypt \
                     --encryption-context purpose=test \
                     --metadata-output ~/metadata \
                     --max-encrypted-data-keys 1 \
                     --buffer \
                     --output ~/output/
```

**Step 8: View the contents of the decrypted file**

Executed the following command:

```
   cat secret1.txt.encrypted.decrypted
```

After successful decryption, I could see the original plaintext contents of the secret1.txt as shown in this [Screenshot](https://github.com/gmonika-blip/my-aws-restart-journey/blob/da44719161aaf5dbad6cba1bc1bbcfc205d1b990/Labs/Security/Data%20Protection%20Using%20Encryption/Decrypt-theEncryptedFile-Lab278.png) .









