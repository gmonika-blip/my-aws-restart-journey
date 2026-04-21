
# Using AWS Systems Manager

## Lab Overview

AWS Systems Manager is a collection of capabilities that one could use to centralize operational data and automate tasks across their Amazon Web Services (AWS) resources. Systems Manager can configure and manage Amazon Elastic Compute Cloud (Amazon EC2) instances, on-premises servers, virtual machines, and other AWS resources at scale.

![AWS Systems Manager](https://github.com/gmonika-blip/my-aws-restart-journey/blob/b15f93fa1f3d9e9cafe31ee51158a04a91b5ee47/Labs/Extra%20Labs/images/AWS-SystemsManager.png)

## Objectives

- Verify configurations and permissions  
- Run tasks on multiple servers  
- Update application settings or configurations  
- Access the command line on an instance 

---

## Task 1: Generated Inventory Lists for Managed Instances

Used **Fleet Manager**, a capability of Systems Manager, to collect operating system information, application information, and metadata from EC2 instances, on-premises servers, or virtual machines in a hybrid environment. Also used Fleet Manager to query metadata and quickly identify which instances were running the required software and configurations, and which instances needed updates.

In this task, I used Fleet Manager to gather inventory from an EC2 instance and performed the following steps:

1. In the AWS Management Console search box, entered **Systems Manager** and pressed **Enter**.
2. In the left navigation pane, under **Node Tools**, chose **Fleet Manager**.
3. Opened the **Account management** dropdown list and chose **Set up inventory**.
4. To create an association that collected software and settings information for the managed instance, I selected the following options:

   - Under **Provide inventory details**, for **Name**, entered:  
     `Inventory-Association`

   - Under **Targets**, selected:
     - **Specify targets by:** *Manually selecting instances*
     - Selected the row for **Managed Instance**
     - Left the remaining options as default

5. Chose **Setup Inventory**.

A banner message appeared indicating **"Setup inventory request succeeded"**, and Systems Manager Inventory began regularly inventorying the instance.

6. Chose the **Node ID** link to open the node overview.
7. Selected the **Inventory** tab.

This tab listed all applications installed on the instance. I reviewed the installed applications and other available inventory options in the **Inventory type** dropdown list.

### Result

Successfully created a Systems Manager inventory association for the instance. Using Inventory, I was able to review and validate software configurations without needing to connect to the instance using SSH.

---

## Task 2: Installed a Custom Application Using Run Command

In this task, I installed a custom web application (**Widget Manufacturing Dashboard**) using **Run Command**, a capability of Systems Manager.

Systems Manager installed the application on an EC2 instance inside a VPC using Run Command. The install script installed the following:

- Apache web server  
- PHP  
- AWS SDK  
- Widget Manufacturing Dashboard web application  

After installation, the script started the web server.

To complete the installation, I followed the steps:

1. In the upper-left corner, expanded the menu icon.
2. Under **Node Tools**, chose **Run Command**.
3. Chose `Run command`.

A list of pre-configured documents appeared.

4. Chose the search icon in the document selection box.
5. From the dropdown options, I selected:

   - **Owner**
   - **Owned by me**

A document appeared.

6. Selected the document. 

The following information appeared:

- **Description:** Install Dashboard App  
- **Document version:** 1 (Default)

Left the document version set to the default.

7. Under **Target selection**,selected **Choose instances manually**.
8. Under **Instances**, selected **Managed Instance**.

The Managed Instance already had the Systems Manager agent installed and registered, allowing it to be selected for Run Command.

9. Under **Output options**, cleared **Enable an S3 bucket**.
10. Expanded the **AWS command line interface command** section to review the CLI command that could be used later in scripts.
11. Chose **Run**.

A banner appeared with the **Command ID**, indicating the command was successfully sent.

After 1–2 minutes, the **Overall status** changed to **Success**. 

### Validated the Application

To validate the application:

1. In the Vocareum console, opened the **Details** dropdown list.
2. Chose **Show**.
3. Copied the **ServerIP** value (public IP address).
4. Opened a new browser tab, pasted the IP address, and pressed **Enter**.

The **Widget Manufacturing Dashboard** application appeared.

## Result

I successfully used Run Command through Systems Manager to install a custom application without needing to remotely access the instance using SSH.

---

## Task 3: Used Parameter Store to Manage Application Settings

Parameter Store, a capability of Systems Manager, provides secure hierarchical storage for configuration data and secrets management. Users can store values such as passwords, database strings, and license codes as parameter values. These values could be stored as plain text or encrypted data and referenced by their unique parameter name.

In this task, I used Parameter Store to store a parameter that activated a feature in the application, following the steps:

1. Kept the Widget Manufacturing Dashboard browser tab open and returned to the AWS Systems Manager tab.
2. In the left navigation pane, under **Application Tools**, chose **Parameter Store**.
3. Chose **Create parameter**.
4. Entered the following values:

   - **Name:** `/dashboard/show-beta-features`
   - **Description:** `Display beta features`
   - **Tier:** Default
   - **Type:** Default
   - **Value:** `True`

5. Chose **Create parameter**.

A banner appeared indicating **"Create parameter request succeeded"**.

The parameter was stored as a hierarchical path in the format:

`/dashboard/<option>`

The application running on Amazon EC2 automatically checked for this parameter, and if it existed, it displayed additional features.

6. Returned to the application browser tab and refreshed the page.

## Result

After refreshing, I observed that three charts were displayed. The application had checked Parameter Store and enabled the beta chart feature.


---

## Task 4: Used Session Manager to Access Instances

Session Manager, a capability of Systems Manager, allows developers to manage EC2 instances through a browser-based shell or the AWS CLI. It provides secure and auditable instance management without opening inbound ports, maintaining bastion hosts, or managing SSH keys.

In this task, I accessed the EC2 instance through Session Manager by performing the following steps:

1. In the left navigation pane, under **Node Tools**, I chose **Session Manager**.
2. Chose **Start session**.
3. Selected **Managed Instance**.
4. Chose **Start session**.

A new session tab opened in the browser.

5. To activate the cursor,clicked inside the session window.
6. Executed the following command:

```bash
ls /var/www/html
```
The output listed the application files installed on the instance.

7. Executed the following commands:

```
   # Get region
   AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
   export AWS_DEFAULT_REGION=${AZ::-1}

   # List information about EC2 instances
   aws ec2 describe-instances
```

The output lists the EC2 instance details for the Managed Instance in JSON format.

## Result

This task demonstrated how to use Session Manager to log in to an instance without using SSH.

>Access can be restricted to Session Manager through AWS Identity and Access Management (IAM) policies, and AWS CloudTrail logs Session Manager >usage. These options provide better security and auditing than traditional SSH access.

---

## Conclusion

In this lab, I used AWS Systems Manager to manage and automate operational tasks across AWS resources. I successfully generated inventory reports using Fleet Manager, installed a custom web application on an EC2 instance using Run Command, managed application configuration settings through Parameter Store, and securely accessed the instance using Session Manager without relying on SSH. 

Overall, the lab demonstrated how Systems Manager can improve efficiency, security, and centralized management when working with EC2 instances and application environments at scale.
