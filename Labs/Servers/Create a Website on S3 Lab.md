# Creating a Website on S3

## Lab Overview
In this lab, AWS Command Line Interface (AWS CLI) commands were used from an Amazon Elastic Compute Cloud (Amazon EC2) instance to:

- Create an Amazon Simple Storage Service (Amazon S3) bucket  
- Create a new AWS Identity and Access Management (IAM) user with full access to Amazon S3  
- Upload files to Amazon S3 to host a simple website for the Café & Bakery  
- Create a batch file to update the static website when local files were modified  

**Objectives**

After completing this lab, the following objectives were achieved:

- AWS CLI commands using IAM and Amazon S3 services were executed  
- A static website was deployed to an S3 bucket  
- A script was created to copy local files to Amazon S3  

---

### Accessing the AWS Management Console
The lab environment was launched, and access to the AWS Management Console was established. The console was opened in a new browser tab, and the session was automatically authenticated.

---

### Task 1: Connect to an Amazon Linux EC2 Instance Using SSM
A connection to the EC2 instance was established using AWS Systems Manager Session Manager.  

- Accessed the `Details` button at the top, then chose `Show`. Copied the values of `AWS Access Key ID`, `AWS Secret Access Key`for later reference.
- The **InstanceSessionUrl** was accessed in a browser  
- A terminal session was opened using `ssm-user`  
- The user context was switched to `ec2-user`  

```bash
sudo su -l ec2-user
pwd
```

>  This was the SSH terminal where commands were executed as instructed throughout the lab.

---

### Task 2: Configure the AWS CLI
The AWS CLI, which was pre-installed on the instance, was configured using provided credentials.

```
  aws configure
```

The following values were entered:

- **AWS Access Key ID**: (Enter value from Task 1)

- **AWS Secret Access Key**: (Enter value from Task 1)

- **Default region name**: `us-west-2`

- **Default output format**: `json`

---

### Task 3: Create an S3 Bucket Using the AWS CLI

The `s3api` command was used to create a new S3 bucket with the AWS credentials provided in the lab. By default, S3 buckets are created in the `us-east-1` Region.

> In this lab, both `s3api` and `s3` commands were used. The `s3` commands were built on top of the operations provided by `s3api`.

When creating a new S3 bucket, a globally unique name is required. A naming convention such as a combination of initials, last name, and random numbers was used (for example: `twhitlock256`).

To create the S3 bucket in the `us-west-2` Region, the following command was executed:

```bash
aws s3api create-bucket \
  --bucket <bucket-name> \
  --region us-west-2 \
  --create-bucket-configuration LocationConstraint=us-west-2
```

The command included:

- The --region us-west-2 parameter
- The --create-bucket-configuration LocationConstraint=us-west-2 parameter

Upon successful execution, a JSON-formatted response was returned containing a Location value that reflected the bucket name, for example:

```
</> JSON
{
  "Location": "http://twhitlock256.s3.amazonaws.com/"
}
```
---

### Task 4: Create a New IAM User with Full Access to Amazon S3

The AWS CLI command `aws iam create-user` was used to create a new IAM user for the AWS account. The `--user-name` option specified a unique username within the account.

A new IAM user named `awsS3user` was created using the following command:

```bash
aws iam create-user --user-name awsS3user
```
A login profile for the new user was then created using the command:
```
</>bash
aws iam create-login-profile --user-name awsS3user --password Training123!
```
