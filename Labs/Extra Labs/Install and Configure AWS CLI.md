# Install and Configure the AWS CLI

**The AWS Command Line Interface (AWS CLI)** is a command line tool that provides an interface for interacting with products and services from Amazon Web Services (AWS). The AWS CLI can be installed on your local machine or a virtual machine such as an Amazon Elastic Compute Cloud (Amazon EC2) instance.

## Lab overview

In this lab, I installed and configured the AWS CLI on a Red Hat Linux instance because this instance type does not have the AWS CLI pre-installed. Some instance types, such as Amazon Linux, do come pre-installed with the AWS CLI. 

During this activity, I established a Secure Shell (SSH) connection to the instance. I configured the installation with an access key that could connect to an AWS account. Finally, I practiced using the AWS CLI to interact with AWS Identity and Access Management (IAM).

**Objectives**

- Install and configure the AWS CLI

- Connect the AWS CLI to an AWS account

- Access IAM by using the AWS CLI

The result of the lab is refelected in the following diagram:

![Architecture Diagram](https://github.com/gmonika-blip/my-aws-restart-journey/blob/c5a925ceb3e0c6ca067853f8ce03550d0d1ef5fd/Labs/Extra%20Labs/images/ArchitectureDiagram.png)

---

## Task 1: Connected to the Red Hat EC2 Instance Using SSH

In this task, the EC2 instance was accessed using SSH.

### Windows Users

The following steps were completed on a Windows system:

- The **Details** drop-down menu was selected and **Show** was chosen to open the Credentials window.
- The **Download PPK** button was selected and the `labsuser.ppk` file was saved (usually in the **Downloads** folder).
- The **PublicIP** address was noted for later use.
- The **Details** panel was closed by selecting the **X**.
- **PuTTY** was downloaded and installed (if it was not already installed).
- `putty.exe` was opened and configured to connect to the EC2 instance using the downloaded `.ppk` file.

### macOS and Linux Users

The following steps were completed on macOS or Linux:

- The **Details** drop-down menu was selected and **Show** was chosen to open the Credentials window.
- The **Download PEM** option was selected and the `labsuser.pem` file was saved.
- The **PublicIP** address was copied into a text editor for later use.
- The **Details** panel was closed by selecting the **X**.
- A terminal window was opened and the directory was changed to the location where the `.pem` file was downloaded:

```bash
cd ~/Downloads
