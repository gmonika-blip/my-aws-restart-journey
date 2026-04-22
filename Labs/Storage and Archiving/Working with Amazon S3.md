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

## Task 2: Creating and Initializing the S3 Share Bucket

In this task, the S3 share bucket was created using the AWS CLI, and sample images were uploaded.

To create an S3 bucket, the following command was used. The bucket name began with `cafe-` and included a unique combination of letters and numbers:

```bash
aws s3 mb s3://<cafe-xxxnnn> --region 'us-west-2'
```

A confirmation message similar to the following was returned:

```
make_bucket: cafe-xxxx9999999
```

Next, sample images were uploaded into the /images prefix from the initial-images folder on the CLI host. The following command was executed:

```
aws s3 sync ~/initial-images/ s3://<cafe-xxxnnn>/images
```

The output listed the uploaded image files.

To verify the upload, the following command was run:

```
aws s3 ls s3://<cafe-xxxnnn>/images/ --human-readable --summarize
```

The uploaded files were displayed along with the total number of files and their combined size.


## Task 3: Reviewing IAM Group and User Permissions

In this task, the permissions assigned to the **mediaco IAM user group** were reviewed. This group had been created to allow media company users to access the AWS Management Console or AWS CLI to upload and manage images in an S3 shared bucket. The group setup simplified user permission management. The permissions inherited by the **mediacouser** IAM user (a member of the group) were also reviewed and tested.


### Task 3.1: Reviewing the mediaco IAM Group

The permissions assigned to the **mediaco group** were reviewed in the IAM console.

### Steps performed:
- The AWS Management Console was opened.
- IAM was searched and selected to open the IAM Management Console.
- In the left navigation pane, **User groups** was selected.
- The **mediaco** group was selected from the list.
- The **Permissions** tab was opened.

### Policy review:
- The **IAMUserChangePassword** policy was expanded.
  - The AWS managed policy allowing users to change their own password was reviewed.
  - The policy was then collapsed.

- The **mediaCoPolicy** was expanded and reviewed.
  - The following permissions were identified:
    - **AllowGroupToSeeBucketListInTheConsole**
      - Allowed users to view the list of S3 buckets in the account via the console.
    - **AllowRootLevelListingOfTheBucket**
      - Allowed users to view first-level objects in the *cafe* bucket.
    - **AllowUserSpecificActionsOnlyInTheSpecificPrefix**
      - Allowed actions (GetObject, PutObject, DeleteObject) on objects in the `cafe-*/images/*` prefix.
      - Included additional version-related permissions for future use.
  - The policy was then collapsed.



### Task 3.2: Reviewing the mediacouser IAM User

The properties and permissions of the **mediacouser** IAM user were reviewed.

### Steps performed:
- IAM console navigation pane was opened.
- **Users** was selected.
- The **mediacouser** user was selected.
- On the **Permissions** tab:
  - Two policies were confirmed:
    - IAMUserChangePassword
    - mediaCoPolicy

### Group membership verification:
- The **Groups** tab was selected.
- It was confirmed that **mediacouser** was a member of the **mediaco group**.
- The user inherited permissions from this group.

### Access key creation:
- The **Security credentials** tab was selected.
- **Create access key** was chosen.
- The following options were selected:
  - Command Line Interface (CLI)
  - Confirmation checkbox acknowledging recommendation
- The access key was created.
- The **.csv file** containing credentials was downloaded.
- The process was completed by selecting **Done**.

### Console sign-in link:
- The **Console sign-in link** was copied for later use.


### Task 3.3: Testing mediacouser Permissions

The permissions of the **mediacouser** were tested by signing in as the user and performing S3 operations.

### Sign-in process:
- A new browser or incognito/private window was used (to avoid logging out of the existing session).
- The copied **Console sign-in link** was opened.
- The following credentials were entered:
  - IAM user name: `mediacouser`
  - Password: `Training1!`
- The user signed in successfully.


### Amazon S3 testing

- The **S3 console** was opened from the AWS Management Console.
- The previously created bucket was selected.
- The **images/** folder was opened.

### View test:
- `Donuts.jpg` was selected and opened.
- The image successfully displayed in a new browser tab.
- The tab was closed afterward.



### Upload test:
- The **Upload** button was selected.
- A local image file was added.
- The file was uploaded successfully.
- The uploaded file was opened and displayed in a new tab.
- The tab was closed.



### Delete test:
- `Cup-of-Hot-Chocolate.jpg` was selected.
- The **Delete** option was chosen.
- `delete` was entered in the confirmation field.
- The object was successfully deleted.


### Unauthorized action test:
- The bucket **Permissions** tab was opened.
- An **Insufficient permissions** error was displayed.
  - It was confirmed that `mediacouser` could not modify bucket permissions.
- An attempt to upload directly to the bucket root would also fail due to restricted permissions.



### Final result:
- The **S3 bucket configuration and IAM policies worked as intended**.
- The **mediacouser** user was able to:
  - View objects
  - Upload objects
  - Delete objects
- The user was correctly restricted from:
  - Modifying bucket permissions


## Task 4: Configuring Event Notifications on the S3 Share Bucket

In this task, the S3 share bucket was configured to generate event notifications whenever its contents changed. These events were published to an Amazon SNS topic, which then sent email notifications to subscribed users. The setup included creating an SNS topic, configuring permissions, subscribing an email endpoint, and linking the S3 bucket to the SNS topic through event notifications.

### Task 4.1: Creating and Configuring the `s3NotificationTopic` SNS Topic

The SNS topic **s3NotificationTopic** was created and configured to receive messages from Amazon S3.

### Steps performed:

- The AWS Management Console was opened using the **voclabs/user** session.
- SNS was searched and the **Simple Notification Service (SNS)** console was opened.
- In the navigation pane, **Topics** was selected.
- **Create topic** was chosen.
- The following configuration was applied:
  - Type: **Standard**
  - Name: `s3NotificationTopic`
- The topic was created successfully.

### ARN retrieval:
- The **ARN (Amazon Resource Name)** of the topic was copied.
- The ARN was saved for later use in the configuration steps.


### Access policy configuration:
- The topic was selected and **Edit** was chosen.
- The **Access policy** section was expanded.
- The JSON policy was updated to allow Amazon S3 to publish messages to the SNS topic.

#### Key policy behavior:
- Allowed the **S3 service** (`s3.amazonaws.com`) to publish messages.
- Restricted publishing to a specific S3 bucket using `aws:SourceArn`.
- Ensured only the designated bucket could send notifications.

- The updated policy was saved successfully.


### Subscription setup:

- The **Subscriptions** tab was opened.
- **Create subscription** was selected.
- The following values were configured:
  - Topic ARN: `s3NotificationTopic`
  - Protocol: **Email**
  - Endpoint: a valid email address

- The subscription was created successfully.


### Email confirmation:
- An email titled **AWS Notification - Subscription Confirmation** was received.
- The **Confirm subscription** link was selected.
- A confirmation page opened showing **Subscription confirmed!**


### Task 4.2: Adding Event Notification Configuration to the S3 Bucket

An event notification configuration was created and associated with the S3 bucket using AWS CLI.

### Configuration file creation:

- A new file was created in the CLI environment:
  ```bash
  vi s3EventNotification.json
  ```
  The editor was switched to insert mode by pressing: `i`
  
  The following JSON configuration was added:
  
  ```
  {
   "TopicConfigurations": [
    {
      "TopicArn": "<ARN of s3NotificationTopic>",
      "Events": ["s3:ObjectCreated:*", "s3:ObjectRemoved:*"],
      "Filter": {
        "Key": {
          "FilterRules": [
            {
              "Name": "prefix",
              "Value": "images/"}
          ]
        }
      }
    }
  ]  }  




