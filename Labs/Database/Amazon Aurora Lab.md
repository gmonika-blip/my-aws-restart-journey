
# Introduction to Amazon Aurora

**Amazon Aurora** is a fully managed, MySQL-compatible, relational database engine that combines the performance and reliability of high-end commercial databases with the simplicity and cost-effectiveness of open-source databases. It delivers up to five times the performance of MySQL without requiring changes to most of your existing applications that use MySQL databases.
 

## Lab Overview

This lab introduced me to Amazon Aurora and provided a basic understanding of how to use it.

**Objectives:**

- Created an Amazon Aurora instance  
- Connected to a pre-created Amazon EC2 instance  
- Configured the EC2 instance to connect to Aurora  
- Queried the Aurora database instance

**Prerequisites:**

- Some experience using the Linux operating system  
- A basic understanding of Structured Query Language (SQL)

<br>

**Technologies used in this Lab:**

-**Amazon Elastic Compute Cloud(Amazon EC2)** is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2 reduces the time required to provision   new server instances to minutes, giving you the ability to quickly scale capacity, both up and down, as your computing requirements change.

-**Amazon Relational Database Service (Amazon RDS)** makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration      tasks, freeing you up to focus on your applications and business. Amazon RDS provides you with six database engines to choose from, including Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL, and MariaDB.


 ## Task 1: Created an Aurora Instance

In this task, I created an Amazon Aurora database (DB) instance and configured it for a basic lab environment.

### Step 1: Navigated to RDS

1. At the top of the AWS Management Console, I searched for and selected **RDS**.  
   > This service is used to create and manage relational databases in AWS.

2. In the left navigation menu, I chose **Databases**.  
   > This section lists all database instances and allows creation of new ones.

3. I selected **Create database**.  
   > This starts the database setup process.



### Step 2: Database Configuration 
I configured the database with the following options.

- **Database creation method:** Standard create  
  > Provides full control over configuration options instead of using quick defaults.

- **Engine type:** Aurora (MySQL Compatible)  
  > Chosen for high performance and compatibility with MySQL.

- **Engine version:** Default for major version 8.0  
  > Ensures stability and compatibility with modern features.

- **Template:** Dev/Test  
  > Optimized for cost-effective, non-production environments.



### Step 3: Settings Configuration

- **DB cluster identifier:** `aurora`  
  > A unique name to identify the database cluster.

- **Master username:** `admin`  
  > The primary login user for database access.

- **Master password:** `admin123`  
  > Password used for authentication.

- **Confirm password:** `admin123`  
  > Ensures the password is entered correctly.



### Step 4: Instance Configuration

- **DB instance class:** Burstable classes (t class)  
  > Suitable for small workloads with occasional spikes in usage.

- **Instance type:** `db.t3.medium`  
  > Provides a balance of cost and performance for this lab.



### Step 5: Availability & Durability

- **Multi-AZ deployment:** Did not create an Aurora Replica  
  > High availability was not required for this lab, helping reduce cost and complexity.



### Step 6: Connectivity Configuration

- **Virtual Private Cloud (VPC):** `LabVPC`  
  > Defines the network where the database is deployed.

- **Subnet group:** `dbsubnetgroup`  
  > Specifies which subnets the database can use within the VPC.

- **Public access:** No  
  > Restricts direct internet access for better security.

- **VPC security group:** Chose existing  
  > Uses predefined firewall rules for controlled access.

- Removed the **default** security group  
  > Avoids overly permissive access rules.

- Selected **DBSecurityGroup**  
  > Ensures only allowed resources (like EC2) can connect.

> 💡 A DB subnet group groups subnets to control database placement and improve security.



### Step 7: Additional Settings

- Disabled **Enhanced monitoring**  
  > Reduces unnecessary monitoring overhead for this lab.

- **Initial database name:** `world`  
  > Creates a default database schema for immediate use.

- Disabled **Encryption**  
  > Simplifies setup since advanced security is not required here.

- Disabled **Auto minor version upgrade**  
  > Prevents automatic updates that might affect lab consistency.



### Step 8: Launching the Database

- I scrolled to the bottom of the page.
- Selected **Create database**.  
   > This initiated the database provisioning process.

> ⏳ The Aurora DB instance took a few minutes to launch.
>
The following is a screenshot showing that the Aurora Database Instance was launched successfully:


![Aurora Instance Launched screenshot](https://github.com/gmonika-blip/my-aws-restart-journey/blob/51d74439f356da2e27389ce77e4b797d30b75517/Labs/Database/images/AuroraInstance.png)


### Result

I successfully created an Amazon Aurora database instance and understood the purpose behind each configuration step.

---

## Task 2: Connect to an Amazon EC2 Linux instance

In this task, I logged into to my Amazon EC2 Linux instance.

**Step 1:** 

- At the top of the AWS Management Console, in the search bar, I searched for and chose EC2.

- In the left navigation menu, chose Instances.

- Next to the instance labelled `Command Host`, selected the check box, and then chose `Connect`.
      For Connect to instance, choose `Session Manager`.
      Choose `Connect` to open a terminal window.


### Result 

I successfully connected to the Amazon EC2 instance named Command Host.

---

## Task 3: Configure the Amazon EC2 Linux instance to connect to Aurora

In this task, I used the yum package manager to install the MariaDB client and then configured the Amazon EC2 Linux instance to connect to the Aurora database.

**Step 1:** Executed the following command to install MariaDB client.

```
sudo yum install mariadb -y
```
The expected output is:
```sql
******************************
**** This is OUTPUT ONLY. ****
******************************

Install  1 Package

Total download size: 8.8 M
Installed size: 49 M
Downloading packages:
mariadb-5.5.68-1.amzn2.0.1.x86_64.rpm                    |  8.8 MB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
    Installing : 1:mariadb-5.5.68-1.amzn2.0.1.86_64       1/1
    Verifying  : 1:mariadb-5.5.68-1.amzn2.0.1.x86_64      1/1

Installed:
mariadb.x86_64 1:5.5.68-1.amzn2.0.1

Complete!   
```






