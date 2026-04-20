# Amazon Elastic Compute Cloud

**Amazon Elastic Compute Cloud (Amazon EC2)** is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. 
Amazon EC2's simple web service interface allows developers to obtain and configure capacity with minimal friction. It provides them with complete control of their computing resources and lets them run on Amazon's proven computing environment. Amazon EC2 reduces the time required to obtain and boot new server instances to minutes, allowing developers to quickly scale capacity, both up and down, as the computing requirements change.


**Key Benefits**

  --Scalable: Adjust compute capacity on demand
  
  --Fast provisioning: Launch instances within minutes
  
  --Cost-effective: Pay only for what you use
  
  --Full control: Configure OS, networking, and security
  
  --Reliable: Build fault-tolerant and resilient applications

 ## Lab Overview
  
  This lab gave me a basic overview of launching, resizing, managing, and monitoring an Amazon EC2 instance.

  **Objectives:**
    
    Launch a web server with termination protection enabled

    Monitor Your EC2 instance

    Modify the security group that your web server is using to allow HTTP access

    Resize your Amazon EC2 instance to scale

    Test termination protection

    Terminate your EC2 instance

## Task 1: Launching the EC2 Instance

In this task, I launched an Amazon EC2 instance with termination protection. Termination protection helps prevent accidental deletion of your instance. I also used a **User Data script** to deploy a simple web server automatically.

### Step 1: Open EC2 Dashboard
1. In the **AWS Management Console**, go to **Services**.
2. Select **EC2**.
3. In the left navigation pane, choose **EC2 Dashboard**.
4. Click **Launch instance**, then select **Launch instance** again.

### Step 2: Name Your EC2 Instance
When you name your instance, AWS creates a key-value pair:
- **Key**: `Name`  
- **Value**: Your chosen name  

1. In the **Name and tags** section:
   - Enter: `Web Server`

### Step 3: Choose an Amazon Machine Image (AMI)
An **Amazon Machine Image (AMI)** provides the configuration required to launch your instance. It includes:
- A template for the root volume (e.g., operating system or application server)
- Launch permissions for AWS accounts
- Storage configuration (block device mapping)

1. Locate the **Application and OS Images (Amazon Machine Image)** section.
2. Under **AMI**, ensure that **Amazon Linux 2023** is selected (default setting).
3. Keep the default selection.

> 💡 The **Quick Start** list contains commonly used AMIs. You can also use custom AMIs or select from the **AWS Marketplace**, an online store where you can buy or sell software that runs on AWS.


### Step 4: Choosing an Instance Type

Amazon EC2 provides a wide selection of instance types optimized for different use cases. Instance types vary in CPU, memory, storage, and networking capacity, allowing you to choose the right configuration for your workload.

1. Open the **Instance type** dropdown menu.
2. Select **t3.micro**.
   - This instance type provides:
     - 2 virtual CPUs
     - 1 GiB of memory

### Step 5: Configuring a Key Pair

Amazon EC2 uses public-key cryptography to securely handle login credentials. Normally, you would create a key pair, download a private key, and use it to connect to your instance.

However, in this lab, you will not need to log in to the instance.

1. In the **Key pair (login)** section:
   - Select **Proceed without a key pair (Not recommended)**.

### Step 5: Configuring Network Settings

The VPC determines which Virtual Private Cloud (VPC) you want to launch the instance into. You can have multiple VPCs for different environments such as development, testing, and production.

1. In the **Network settings** pane, choose **Edit**.
2. For **VPC (required)**, select **Lab VPC**.

#### Configure Security Group
A security group acts as a virtual firewall that controls traffic for one or more instances. When you launch an instance, you associate one or more security groups with it. Rules added to a security group control inbound and outbound traffic, and changes are automatically applied to all associated instances.

1. Still in **Network settings**, configure the security group as follows:
   - **Security group name (required):** `Web Server security group`  
   - **Description:** `Security group for my web server`

2. Under **Inbound security group rules**, select **Remove** to delete the SSH rule.

> 🔒 In this lab, SSH access is not required. Removing it improves the security of the instance.


### Step 6: Adding Storage

Amazon EC2 stores data using a network-attached virtual disk called **Amazon Elastic Block Store (Amazon EBS)**.

1. By default, the instance is launched with an **8 GiB root volume** (boot volume).
2. In the **Configure storage** section, keep the default settings unchanged.

> 💡 The root volume contains the operating system and is required for the instance to boot.
>
> ### Step 7: Configuring Advanced Details

1. Expand the **Advanced details** pane.
2. Find **Termination protection** and select the dropdown.
3. Choose **Enable**.

When you launch an instance in Amazon EC2, you can pass **User data** to automate configuration tasks and run scripts after the instance starts.

#### Add User Data Script

1. Locate the **User data** text box.
2. Copy and paste the following script:

```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

The script does the following:

--Install an Apache web server (httpd)

--Configure the web server to automatically start on boot

--Activate the Web server

--Create a simple web page

  
  


