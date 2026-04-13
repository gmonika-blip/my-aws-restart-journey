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
