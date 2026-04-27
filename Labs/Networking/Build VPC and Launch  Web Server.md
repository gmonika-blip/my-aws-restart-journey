# Build a VPC and Launch a Web Server

## AWS VPC (Amazon Virtual Private Cloud)

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


## Lab Scenario
In this lab, I used Amazon Virtual Private Cloud (VPC) to create a VPC and added additional components to produce a customized network for a Fortune 100 customer. I also created security groups for my EC2 instance. I then configured and customized an EC2 instance to run the web server and launched it into the VPC that looked like the following customer diagram:
