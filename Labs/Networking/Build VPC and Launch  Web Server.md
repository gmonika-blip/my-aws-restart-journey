# Build a VPC and Launch a Web Server

An **AWS VPC** (Amazon Virtual Private Cloud) is a customizable, isolated network within AWS where you can run cloud resources like servers, databases, and applications securely. In simple terms, a VPC is your own private network in the AWS cloud.

### What you control in an AWS VPC

- **IP address range (CIDR block):** Define the size of your network  
- **Subnets:** Segment your network (e.g., public and private)  
- **Route tables:** Control how traffic flows  
- **Internet Gateway:** Enable internet access  
- **Security Groups & Network ACLs:** Manage access with firewall-like rules  

### Example setup

- Deploy a web server in a **public subnet** (internet-facing)  
- Place a database in a **private subnet** (no direct internet access)  
- Use security rules to control communication between them  

### Why AWS VPC is important

- **Isolation:** Separate your resources from others  
- **Security:** Fine-grained control over traffic  
- **Flexibility:** Support simple to complex architectures  
- **Hybrid connectivity:** Connect to on-premises via VPN or Direct Connect  

> AWS VPC is the foundation of networking in AWS, enabling secure and scalable cloud infrastructure.

---

## Lab Scenario
In this lab, I used Amazon Virtual Private Cloud (VPC) to create a VPC and added additional components to produce a customized network for a Fortune 100 customer. I also created security groups for my EC2 instance. I then configured and customized an EC2 instance to run the web server and launched it into the VPC that looked like the following customer diagram:

**Objectives**

After completing this lab, I will be able to:

- Create a Virtual Private Cloud (VPC)  
- Create and configure subnets  
- Configure a security group  
- Launch an Amazon EC2 instance within a VPC  

### Accessing the AWS Management Console

1. At the top of the lab instructions, I chose **Start Lab**  
2. A *Start Lab* panel opened and displayed the lab status  

> 💡 **Tip:** If you need more time, choose **Start Lab** again to reset the timer  

3. Waited until the status shows **Lab status: ready**  
4. Closed the panel by selecting **X**  
5. Chose **AWS** at the top of the instructions  

This opened the **AWS Management Console** in a new browser tab and automatically signed me in.

> 💡 **Tip:** The new tab did not open as my browser was blocking pop-ups.  
> I allowed pop-ups using the browser notification or icon, then I tried again.


### Task 1: Create Your VPC

I used the VPC Wizard to create a VPC along with an internet gateway, subnets, and a NAT gateway.

An **internet gateway** allows communication between a VPC and the internet.  

Subnets are isolated network segments within a VPC:
- A **public subnet** has a route to the internet gateway  
- A **private subnet** does not have direct internet access  

A **NAT gateway**, enables instances in private subnets to access the internet securely.

### Steps

1. Open the **AWS Management Console**
2. Search for **VPC** in the top search bar and select it
3. In the VPC dashboard, choose **Create VPC**

### Configuration

Set the following options:

- **Resources to create:** VPC and more  
- **Name tag auto-generation:** Uncheck *Auto-generate*  
- **IPv4 CIDR:** `10.0.0.0/16`  
- **IPv6 CIDR block:** No IPv6 CIDR block  
- **Tenancy:** Default  
- **Availability Zones (AZs):** 1  
- **Public subnets:** 1  
- **Private subnets:** 1  

#### Customize subnet CIDR blocks

- **Public subnet (us-west-2a):** `10.0.0.0/24`  
- **Private subnet (us-west-2a):** `10.0.1.0/24`  

- **NAT gateways:** In 1 AZ  
- **VPC endpoints:** None  

### Resource Naming

In the preview pane, assign the following names:

- **VPC:** Lab VPC  

**Subnets:**
- Public Subnet 1  
- Private Subnet 1  

**Route Tables:**
- Public Route Table  
- Private Route Table  

### Finalize

1. Choose **Create VPC**  
2. Wait for the success message  
3. Select **View VPC** to review your configuration  

Your **Lab VPC** is now created and ready for use.
