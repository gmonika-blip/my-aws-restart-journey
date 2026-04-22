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

### Architecture Diagram

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


### Accessing the AWS Management Console

- The lab was started by selecting **Start Lab**  
- After the status changed to *"Lab status: ready"*, the panel was closed  

- The **AWS Management Console** was opened in a new browser tab, and the session was automatically signed in  

> If the tab did not open, pop-ups were enabled in the browser

- The console was arranged alongside the lab instructions for easier navigation  

- The **Details** section was opened, and **Show** was selected  
- From the **Credentials** panel, the **AccessKey** and **SecretKey** values were copied and saved in a text editor for later use
> **Note:** These credentials were used later when configuring the AWS CLI in Task 1.2
  
## Task 1: Connecting to the CLI Host EC2 Instance and Configuring the AWS CLI

In this task, a connection was established to the CLI Host EC2 instance using EC2 Instance Connect, and the AWS CLI was configured to run commands.

### Task 1.1: Connecting to the CLI Host EC2 Instance

- The AWS Management Console was opened, and **EC2** was searched and selected  
- In the navigation pane, **Instances** was chosen  
- The **CLI Host** instance was selected from the list  
- The **Connect** option was chosen  
- Under the **EC2 Instance Connect** tab, **Connect** was selected  

A new browser tab opened with the EC2 Instance Connect terminal, which was used throughout the lab.

> If the terminal became unresponsive, the browser was refreshed or the connection steps were repeated.

### Task 1.2: Configuring the AWS CLI

The AWS CLI profile was configured by running:

```bash
aws configure
```

The following values were entered when prompted:

- AWS Access Key ID: AccessKey (see [Accessing the AWS Management Console](#accessing-the-aws-management-console))  
- AWS Secret Access Key: SecretKey (see [Accessing the AWS Management Console](#accessing-the-aws-management-console))  
- Default region name: `us-west-2`
- Default output format: `json`

The AWS CLI was then ready to be used to interact with AWS services.



