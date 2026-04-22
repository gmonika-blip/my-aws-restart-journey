# Working with Amazon S3

**Amazon S3 (Amazon Simple Storage Service)** is a cloud-based object storage service offered by Amazon Web Services.  
 It allows you to store and retrieve any amount of data from anywhere on the internet.

### Key Features

- **Scalablity**: Store data from small files to exabytes with automatic scaling  
- **Durablity & Availability**: 99.999999999% durability and 99.99% availability  
- **Security**: Encryption, access control, and auditing capabilities  
- **Cost-Effective**: Pay only for what you use with multiple storage classes  


### Common Use Cases

- **Backup & Storage**: Store application data, backups, and disaster recovery files  
- **Data Lakes & Analytics**: Store massive datasets for analysis  
- **Static Website Hosting**: Serve static content such as HTML, CSS, and images  
- **Cloud-Native Applications**: Store assets for mobile and web applications

## Lab Overview

In this lab, an Amazon S3 bucket was created and configured to share images with an external user (`mediacouser`) from a media company. The bucket was also configured to send email notifications to an administrator whenever its contents were modified.

### Architecture Summary

The following diagram shows the component architecture of the Amazon S3 file-sharing solution and illustrates its usage flow.

<img src="images/Architecture-Amazon S3.png" alt="App Screenshot" width="50%">

<br>

An AWS IAM user named `mediacouser` had been pre-created with the required Amazon S3 permissions to upload, modify, or delete images in the bucket. Permissions were reviewed to ensure secure and appropriate access.

### Workflow

1. When new product images became available or existing ones needed updates, the media company representative signed in to the AWS Management Console as `mediacouser` and managed the bucket contents.

2. Alternatively, the user used the AWS CLI to upload, modify, or delete files in the S3 bucket.

3. When changes were detected, Amazon S3 published a notification to the `s3NotificationTopic` (Amazon SNS topic).

4. The administrator subscribed to the topic received an email with details about the changes.


> **Note:** In real-world scenarios, external users typically would not have direct access to a CLI host as shown in the architecture diagram.

### Objectives

By the end of this lab, the following tasks were completed:

- Used `s3api` and `s3` AWS CLI commands to create and configure an S3 bucket  
- Verified write permissions for a user on the S3 bucket  
- Configured event notifications for the S3 bucket  



