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

Reviewed information under the following tabs:
- **Details**
- **Security**
- **Networking**

#### Final Status Check

Waited until my instance showed the following:

- **Instance State:** `Running`
- **Status Checks:** `2/2 checks passed`

> 🔄 Refreshed the page if necessary until both conditions were met.
>
### Result

I was able to successfully lauch my first EC2 instance

---

## Task 3: Updated Security Group and Accessed the Web Server

When I launched the Amazon EC2 instance, I provided a script that installed a web server and created a simple web page. In this task, I accessed content from the web server.

1. Selected the instance by checking the box and opened the **Details** tab.
2. Copied the **Public IPv4 address** of the instance to my clipboard.
3. Opened a new tab in my web browser, pasted the IP address, and pressed **Enter**.

### Question: Was I able to access the web server? Why not?

`
I was not able to access the web server because the security group did not permit inbound traffic on **port 80 (HTTP)**. This demonstrated how a security group acts as a firewall to control inbound and outbound traffic for an instance.
`

### Updating the Security Group

To fix this issue, I updated the security group to allow HTTP traffic.

1. Returned to the **EC2 Management Console** tab.
2. In the left navigation pane, selected **Security Groups** under **Network & Security**.
3. Selected **Web Server security group**.
4. Opened the **Inbound rules** tab.

At this point, the security group had no inbound rules.

5. Selected **Edit inbound rules**.
6. Selected **Add rule** and configured it as follows:
   - **Type:** HTTP  
   - **Source:** Anywhere-IPv4  

7. Selected **Save rules**.


### Verifying Access

1. Returned to the browser tab containing the web server.
2. Refreshed the page.

The following message was displayed:

> **Hello From Your Web Server!**

### Result

I successfully modified the security group to allow HTTP traffic into my Amazon EC2 instance.

---

## Task 4: Resized the Instance (Instance Type and EBS Volume)

As my needs changed, I observed that the instance could become over-utilized (too small) or under-utilized (too large). In such cases, I could change the instance type. For example, a `t3.micro` instance could be changed to a `t3.small` instance. I could also modify the size of an attached disk.

### Stopping the Instance

Before resizing the instance, I stopped it.

> When I stopped an instance, it was shut down. There was no charge for a stopped instance, but storage charges for attached Amazon EBS volumes still applied.

1. On the Amazon EC2 Management Console, I selected **Instances** from the left navigation pane.
2. Ensured that **Web Server** was selected.
3. Selected **Instance state** → **Stop instance**.
4. Confirmed by selecting **Stop**.

The instance performed a normal shutdown and then stopped running.

5. I waited until the **Instance State** showed: `stopped`.


### Changing the Instance Type

After the instance had stopped, I changed its instance type.

1. Opened the **Actions** menu.
2. Selected **Instance settings** → **Change instance type**.
3. Configured the following:
   - **Instance Type:** `t3.small`

4. Selected **Change instance type**.

> ⚠️ Note: In this lab, I may have been restricted from selecting other instance types.

 ### Resize the EBS Volume

In the left navigation menu, I selected **Volumes** under **Elastic Block Store**.

1. Selected the volume by checking the box.
2. Opened the **Actions** menu and selected **Modify Volume**.

The disk volume was originally **8 GiB**. I increased its size as follows:

- **New size:** `10 GiB`  
  > ⚠️ Note: In this lab, I may have been restricted from creating larger Amazon EBS volumes.

3. Selected **Modify**.
4. Confirmed the change by selecting **Modify** again to increase the volume size.



### Starting the Resized Instance

After resizing the volume, I started the instance again so it could use the updated resources.

1. In the left navigation pane, I selected **Instances**.
2. Selected the **Web Server** instance by checking the box.
3. Navigated to **Instance state** → **Start instance**.

### Result

I successfully resized my Amazon EC2 instance.

- I changed the instance type from `t3.micro` to `t3.small`.
- I increased the root EBS volume from **8 GiB** to **10 GiB**.

> The instance was successfully resized with improved compute and storage capacity.
>
> ---
>
 ## Task 5: Tested Termination Protection

You can delete your instance when you no longer need it. This is referred to as terminating the instance. Once an instance is terminated, it cannot be restarted or reconnected.

In this task, I learned how termination protection prevents accidental deletion of an instance.

### Attempting to Terminate the Instance

1. In the left navigation pane, I selected **Instances**.
2. Selected the **Web Server** instance by checking the box.
3. Opened the **Instance state** menu and selected **Terminate (delete) instance**.

> ⚠️ Note: A warning message appeared stating that for an EBS-backed instance, the root volume would be deleted upon termination, and all data on local drives would be lost. I was prompted to confirm termination.

However, the instance did not terminate. A red error message appeared:
`
 Failed to terminate an instance: The instance may not be terminated.
 `

> 
This occurred because **termination protection was enabled**.



### Disabling Termination Protection

To allow termination, I first disabled termination protection.

1. In the **Actions** menu, I selected **Instance settings** → **Change termination protection**.
2. Unchecked **Enable**.
3. Selected **Save**.



### Terminating the Instance

After disabling termination protection, I was able to terminate the instance.

1. Opened the **Actions** menu again.
2. Selected **Instance state** → **Terminate instance**.
3. Confirmed by selecting **Terminate**.



### Result

I successfully tested termination protection and then terminated my Amazon EC2 instance.

---


  
  


