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

With AWS KMS, you can create and manage cryptographic keys and control their use across a wide range of AWS services and in your applications. AWS KMS is a secure and resilient service that uses hardware security modules (HSMs) that have been validated under the Federal Information Processing Standard (FIPS) Publication 140-2, or are in the process of being validated, to protect your keys.
