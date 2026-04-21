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
```

- Permissions were updated to make the key read-only:
```
chmod 400 labsuser.pem
```

- SSH was used to connect to the EC2 instance (with <ip-address> replaced by the Public IP):

```
ssh -i labsuser.pem ec2-user@<ip-address>
```

When prompted, yes was entered to confirm the connection.

---

## Task 2: Installed the AWS CLI on a Red Hat Linux Instance

In this task, the AWS CLI was installed on a Red Hat Linux EC2 instance using the terminal.

First, the AWS CLI installer file was downloaded into the current directory using the `curl` command:

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```
Next, the downloaded ZIP file was extracted using the unzip command. The -u option was used to overwrite existing files without prompting:

```
unzip -u awscliv2.zip
```

After the files were extracted, the AWS CLI installation script was executed. The sudo command was used to grant the necessary permissions:

```
sudo ./aws/install
```

To confirm that the AWS CLI was installed successfully, the version was checked using the following command:

```
aws --version
```
Finally, the AWS CLI help command was executed to verify that the AWS CLI was working correctly:

```
aws help
```

At the : prompt, q was entered to exit the help menu.

---

## Task 3: Observed IAM Configuration Details in the AWS Management Console

In this task, IAM configuration details for the EC2 instance were reviewed using the AWS Management Console.

1. **IAM** was entered into the AWS Management Console search bar and the **IAM** service was selected to open the IAM console page.

2. **Users** was selected from the navigation pane, and the **awsstudent** user was chosen.

3. Under the **Permissions** tab, the `lab_policy` policy was located. The arrow icon next to `lab_policy` was selected, and the **JSON ({})** button was chosen to view the policy document.

   The `lab_policy` document was displayed in JSON format and showed the permissions granted to the `awsstudent` user for specific AWS services.

4. **Security credentials** tab was selected. In the **Access keys** section, the access key ID for the `awsstudent` user was located.

> **Note:** The secret access key could only be viewed at the time it was created. For this lab, both the access key ID and secret access key were available in the **Details** dropdown list.

---

## Task 4: Configured the AWS CLI to Connect to the AWS Account

In this task, the AWS CLI was configured through the SSH terminal session.

The following command was executed to begin configuration:

```bash
aws configure
```

At the prompts, the AWS CLI was configured using the following values:

- **AWS Access Key ID:** The **Details** dropdown list was selected, **Show** was chosen, and the **AccessKey** value was copied and pasted into the terminal.
- **AWS Secret Access Key:** The **SecretKey** value was copied and pasted into the terminal.
- **Default region name:** `us-west-2` was entered.
- **Default output format:** `json` was entered.

---

## Task 5: Observed IAM Configuration Details Using the AWS CLI

In this task, IAM configuration details for the EC2 instance were verified using the AWS CLI.

In the terminal window, the IAM configuration was tested by running the following command:

```bash
aws iam list-users
```

The output is shown in the following screenshot:

![IAM Configuration](https://github.com/gmonika-blip/my-aws-restart-journey/blob/0db0b3d807b4bab8ab66a66d272bbef455aaa5a6/Labs/Extra%20Labs/images/IAM-Details-CLI.png)

<br>

## Activity Challenge

In this challenge, the AWS CLI Command Reference documentation and the AWS CLI were used to download the `lab_policy` document as a JSON-formatted IAM policy document. This policy document was the same one that was viewed earlier in the AWS Management Console.

Activity challenge solution

I used the following command to list IAM policies and filter customer managed policies:

```
aws iam list-policies --scope Local
```
Next, I used the version number `Arn` information and `DefaultVersionId` found inside the `lab_policy document` to retrieve the JSON IAM policy. Used the > command to save the file. 

```
 aws iam get-policy-version --policy-arn arn:aws:iam::038946776283:policy/lab_policy --version-id v1 > lab_policy.json
```

---

## Conclusion

In this lab, the AWS CLI was successfully installed on a Red Hat Linux instance and connected to an AWS account. The AWS CLI was then used to retrieve IAM policy information by referencing AWS documentation.

**Key Takeaways**

- The AWS CLI was used to manage and control multiple AWS services through the command line, and the same tasks could also be completed using the AWS Management Console.
- To connect to the AWS account using the AWS CLI, an access key ID and secret access key were required. To sign in to the AWS Management Console, a username and password were required.
