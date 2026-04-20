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

### Step 1: Opened EC2 Dashboard
1. In the **AWS Management Console**, navigated to **Services**.
2. Selected **EC2**.
3. In the left navigation pane, chose **EC2 Dashboard**.
4. Clicked **Launch instance**, then selected **Launch instance** again.

### Step 2: Named My EC2 Instance
AWS created a key-value pair:
- **Key**: `Name`  
- **Value**: Your chosen name  

1. In the **Name and tags** section:
   - Entered: `Web Server`

### Step 3: Chose an Amazon Machine Image (AMI)
An **Amazon Machine Image (AMI)** provides the configuration required to launch your instance. It includes:
- A template for the root volume (e.g., operating system or application server)
- Launch permissions for AWS accounts
- Storage configuration (block device mapping)

1. Located the **Application and OS Images (Amazon Machine Image)** section.
2. Under **AMI**, ensured that **Amazon Linux 2023** is selected (default setting).
3. Kept the default selection.

> 💡 The **Quick Start** list contains commonly used AMIs. Developers can also use custom AMIs or select from the **AWS Marketplace**, an online store where they can buy or sell software that runs on AWS.


### Step 4: Chose an Instance Type

Amazon EC2 provides a wide selection of instance types optimized for different use cases. Instance types vary in CPU, memory, storage, and networking capacity, allowing you to choose the right configuration for your workload.

1. Opened the **Instance type** dropdown menu.
2. Selected **t3.micro**.
   - This instance type provides:
     - 2 virtual CPUs
     - 1 GiB of memory

### Step 5: Configured a Key Pair

Amazon EC2 uses public-key cryptography to securely handle login credentials. Normally, I would create a key pair, download a private key, and use it to connect to your instance.

However, in this lab, I was not required to log in to the instance.

1. In the **Key pair (login)** section:
   - Selected **Proceed without a key pair (Not recommended)**.

### Step 5: Configured Network Settings

The VPC determines which Virtual Private Cloud (VPC) I chose to launch the instance into. Developers can have multiple VPCs for different environments such as development, testing, and production.

1. In the **Network settings** pane, I chose **Edit**.
2. For **VPC (required)**, selected **Lab VPC**.

#### Configured Security Group
A security group acts as a virtual firewall that controls traffic for one or more instances. When I launch an instance, I can associate one or more security groups with it. Rules added to a security group control inbound and outbound traffic, and changes are automatically applied to all associated instances.

1. Still in **Network settings**, configured the security group as follows:
   - **Security group name (required):** `Web Server security group`  
   - **Description:** `Security group for my web server`

2. Under **Inbound security group rules**, selected **Remove** to delete the SSH rule.

> 🔒 In this lab, SSH access is not required. Removing it improves the security of the instance.


### Step 6: Adding Storage

Amazon EC2 stores data using a network-attached virtual disk called **Amazon Elastic Block Store (Amazon EBS)**.

1. By default, the instance is launched with an **8 GiB root volume** (boot volume).
2. In the **Configure storage** section, kept the default settings unchanged.

> 💡 The root volume contains the operating system and is required for the instance to boot.
>
### Step 7: Configured Advanced Details

1. Expanded the **Advanced details** pane.
2. Found **Termination protection** and selected the dropdown.
3. Chose **Enable**.

When an instance in Amazon EC2 is launched, deveopers can pass **User data** to automate configuration tasks and run scripts after the instance starts.

#### Added User Data Script

1. Located the **User data** text box.
2. Performed Copy and Paste for the following script:

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

### Step 8: Launched an EC2 Instance

Once configured my instance settings in Amazon EC2, I was ready to launch it.

1. In the right pane, chose **Launch instance**.
2. Selected **View all instances**.

#### Instance Launch Status

After launching, the instance went through the following states:

- **Pending**: The instance is being created and launched.
- **Running**: The instance has started booting and is active.

> ⏳ There was a short delay before the instance became fully accessible.

The instance is automatically assigned a **public DNS name**, which can be used to access it over the internet.

#### Viewing Instance Details

1. Selected the checkbox next to your **Web Server** instance.
2. The **Details** tab displayed detailed information about the instance.

#### Reviewed Instance Information

Checked the following tabs:
- **Details**
- **Security**
- **Networking**

#### Final Status Check

Waited until my instance showed the following:

- **Instance State:** `Running`
- **Status Checks:** `2/2 checks passed`

> 🔄 Refreshed the page if necessary until both conditions were met.


  
  


