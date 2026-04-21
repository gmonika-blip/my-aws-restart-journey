
Using AWS Systems Manager
Lab overview
AWS Systems Manager is a collection of capabilities that you can use to centralize operational data and automate tasks across your Amazon Web Services (AWS) resources. Systems Manager can configure and manage Amazon Elastic Compute Cloud (Amazon EC2) instances, on-premises servers, virtual machines, and other AWS resources at scale. 

Objectives
After completing this lab, you should be able to use Systems Manager to do the following:

Verify configurations and permissions.
Run tasks on multiple servers.
Update application settings or configurations.
Access the command line on an instance.

# Using AWS Systems Manager

## Lab Overview

AWS Systems Manager was a collection of capabilities that you could use to centralize operational data and automate tasks across your Amazon Web Services (AWS) resources. Systems Manager could configure and manage Amazon Elastic Compute Cloud (Amazon EC2) instances, on-premises servers, virtual machines, and other AWS resources at scale.

## Objectives

After completing this lab, you were able to use Systems Manager to do the following:

- Verified configurations and permissions  
- Ran tasks on multiple servers  
- Updated application settings or configurations  
- Accessed the command line on an instance  

## Duration

This activity required approximately **30 minutes** to complete.

---

## Accessing the AWS Management Console

At the top of the instructions, you chose **Start Lab** to launch the lab.

You waited until the message **"Lab status: ready"** appeared, and then chose **X** to close the Start Lab panel.

Next to **Start Lab**, you chose **AWS** to open the AWS Management Console in a new browser tab. The system automatically signed you in.

> **Tip:** If a new browser tab did not open, a banner or icon at the top of the browser indicated that pop-up windows were being blocked. You selected the banner or icon and allowed pop-ups.

You arranged the AWS Management Console so that it appeared alongside the lab instructions.

> **Note:** You did not change the AWS Region unless instructed to do so.

---

## Task 1: Generated Inventory Lists for Managed Instances

You used **Fleet Manager**, a capability of Systems Manager, to collect operating system information, application information, and metadata from EC2 instances, on-premises servers, or virtual machines in a hybrid environment. You also used Fleet Manager to query metadata and quickly identify which instances were running the required software and configurations, and which instances needed updates.

In this task, you used Fleet Manager to gather inventory from an EC2 instance.

1. In the AWS Management Console search box, you entered **Systems Manager** and pressed **Enter**.
2. In the left navigation pane, under **Node Management**, you chose **Fleet Manager**.
3. You opened the **Account management** dropdown list and chose **Set up inventory**.
4. To create an association that collected software and settings information for the managed instance, you selected the following options:

   - Under **Provide inventory details**, for **Name**, you entered:  
     `Inventory-Association`

   - Under **Targets**, you selected:
     - **Specify targets by:** *Manually selecting instances*
     - You selected the row for **Managed Instance**
     - You left the remaining options as default

5. You chose **Setup Inventory**.

A banner message appeared indicating **"Setup inventory request succeeded"**, and Systems Manager Inventory began regularly inventorying the instance.

6. You chose the **Node ID** link to open the node overview.
7. You selected the **Inventory** tab.

This tab listed all applications installed on the instance. You reviewed the installed applications and other available inventory options in the **Inventory type** dropdown list.

You successfully created a Systems Manager inventory association for the instance. Using Inventory, you were able to review and validate software configurations without needing to connect to the instance using SSH.

---

## Task 2: Installed a Custom Application Using Run Command

In this task, you installed a custom web application (**Widget Manufacturing Dashboard**) using **Run Command**, a capability of Systems Manager.

Systems Manager installed the application on an EC2 instance inside a VPC using Run Command. The install script installed the following:

- Apache web server  
- PHP  
- AWS SDK  
- Widget Manufacturing Dashboard web application  

After installation, the script started the web server.

To complete the installation:

1. In the upper-left corner, you expanded the menu icon.
2. Under **Node Management**, you chose **Run Command**.
3. You chose **Run command**.

A list of pre-configured documents appeared.

4. You chose the search icon in the document selection box.
5. From the dropdown options, you selected:

   - **Owner**
   - **Owned by me**

A document appeared.

> **Note:** You did not manually type *Owner* or *Owned by me*, because typing these values did not return results.

6. You selected the document (if it was not already selected).

The following information appeared:

- **Description:** Install Dashboard App  
- **Document version:** 1 (Default)

You left the document version set to the default.

7. Under **Target selection**, you selected **Choose instances manually**.
8. Under **Instances**, you selected **Managed Instance**.

The Managed Instance already had the Systems Manager agent installed and registered, allowing it to be selected for Run Command.

9. Under **Output options**, you cleared **Enable an S3 bucket**.
10. You expanded the **AWS command line interface command** section to review the CLI command that could be used later in scripts.
11. You chose **Run**.

A banner appeared with the **Command ID**, indicating the command was successfully sent.

After 1–2 minutes, the **Overall status** changed to **Success**. If it did not update, you chose the refresh icon.

### Validated the Application

To validate the application:

1. In the Vocareum console, you opened the **Details** dropdown list.
2. You chose **Show**.
3. You copied the **ServerIP** value (public IP address).
4. You opened a new browser tab, pasted the IP address, and pressed **Enter**.

The **Widget Manufacturing Dashboard** application appeared.

You successfully used Run Command through Systems Manager to install a custom application without needing to remotely access the instance using SSH.

---

## Task 3: Used Parameter Store to Manage Application Settings

Parameter Store, a capability of Systems Manager, provided secure hierarchical storage for configuration data and secrets management. You stored values such as passwords, database strings, and license codes as parameter values. These values could be stored as plain text or encrypted data and referenced by their unique parameter name.

In this task, you used Parameter Store to store a parameter that activated a feature in the application.

1. You kept the Widget Manufacturing Dashboard browser tab open and returned to the AWS Systems Manager tab.
2. In the left navigation pane, under **Application Management**, you chose **Parameter Store**.
3. You chose **Create parameter**.
4. You entered the following values:

   - **Name:** `/dashboard/show-beta-features`
   - **Description:** `Display beta features`
   - **Tier:** Default
   - **Type:** Default
   - **Value:** `True`

5. You chose **Create parameter**.

A banner appeared indicating **"Create parameter request succeeded"**.

The parameter was stored as a hierarchical path in the format:

`/dashboard/<option>`

The application running on Amazon EC2 automatically checked for this parameter, and if it existed, it displayed additional features.

6. You returned to the application browser tab and refreshed the page.

If the tab had been closed, you reopened it by copying the **ServerIP** value again from the lab details.

After refreshing, you observed that three charts were displayed. The application had checked Parameter Store and enabled the beta chart feature.

> It was common to configure applications to display "dark features" that were installed but not yet activated.

### Optional Step

You optionally deleted the parameter and refreshed the browser tab again. The third chart disappeared.

---

## Task 4: Used Session Manager to Access Instances

Session Manager, a capability of Systems Manager, allowed you to manage EC2 instances through a browser-based shell or the AWS CLI. It provided secure and auditable instance management without opening inbound ports, maintaining bastion hosts, or managing SSH keys.

In this task, you accessed the EC2 instance through Session Manager.

1. In the left navigation pane, under **Node Management**, you chose **Session Manager**.
2. You chose **Start session**.
3. You selected **Managed Instance**.
4. You chose **Start session**.

A new session tab opened in the browser.

5. To activate the cursor, you clicked inside the session window.
6. You ran the following command:

```bash
ls /var/www/html
