# Working with Amazon EBS (Elastic Block Store)

**Amazon EBS (Elastic Block Store)** is a cloud-based block storage service provided by Amazon Web Services for use with its virtual servers (Amazon EC2 instances). It is like a virtual hard drive in the cloud. It lets you store data that needs to persist even when an EC2 instance is stopped or restarted.

### Amazon EBS Key Benefits

- **Scalability:** Quickly scale storage performance and capacity to support enterprise-grade applications without disrupting operations.

- **High performance:** Provides high availability and durability, including replication within Availability Zones and up to 99.999% durability with io2 Block Express volumes.

- **Optimize storage and cost:** Choose different volume types to balance cost and performance, from low-cost general-purpose storage to high-IOPS, high-throughput options.

- **Security:** Encrypts data at rest and in transit while allowing you to control access and avoid managing your own encryption key infrastructure.

- **Simple data protection:** EBS Snapshots provide point-in-time backups that support disaster recovery, data migration, and compliance requirements.


### Amazon EBS Use Cases

- **Databases**: Supports low-latency storage for SQL and NoSQL databases.  
- **Boot volumes**: Provides persistent root storage for EC2 instances.  
- **Enterprise applications**: Runs critical systems like ERP and CRM.  
- **Big data workloads**: Stores large datasets for analytics and processing.  
- **Development and testing**: Quickly creates and deletes test environments.  
- **Backups and recovery**: Uses snapshots for disaster recovery via Amazon S3.


## Lab overview

In this lab, I learnt how to create an EBS volume and perform operations on it, such as attaching it to an instance, creating a file system, and taking a snapshot backup.

**Objectives:**

- Create an EBS volume.
- Attach and mount an EBS volume to an EC2 instance.
- Create a snapshot of an EBS volume.
- Create an EBS volume from a snapshot.

---

# Task 1: Creating a new EBS volume

In this task, I created and attached an EBS volume to a new EC2 instance.

- On the AWS Management Console, search for and choose **EC2** to open the EC2 Management Console.  
- In the left navigation pane, choose **Instances**.  
- An EC2 instance named **Lab** has already been launched for your lab.  
- Note the **Availability Zone** of the Lab instance (e.g., `us-west-2a`).  
- In the left navigation pane, under **Elastic Block Store**, choose **Volumes**.  
- You will see an existing 8 GiB volume already attached to the instance.  
- Choose **Create volume** and configure:  
  - Volume type: **General Purpose SSD (gp2)**  
  - Size (GiB): **1**  
  - Availability Zone: same as Lab instance (e.g., `us-west-2a`)  
- In **Tags (optional)**, add:  
  - Key: `Name`  
  - Value: `My Volume`  
- Choose **Create volume**.  
- Wait until the volume state changes from **Creating** to **Available**.

---

# Task 2: Attaching the volume to an EC2 instance

- Select **My Volume**.  
- From the **Actions** menu, choose **Attach volume**.  
- Select the **Lab** instance from the Instance dropdown.  
- Set Device name to `/dev/sdb`.  
- Choose **Attach volume**.  
- The volume state will change to **In-use**.

---

# Task 3: Connecting to the Lab EC2 instance

- Search for and choose **EC2** in the AWS Management Console.  
- In the navigation pane, choose **Instances**.  
- Select the **Lab** instance from the list.  
- Choose **Connect**.  
- Open the **EC2 Instance Connect** tab and click **Connect**.  
- A new browser tab opens with a terminal session.  
- Use this terminal for all lab tasks; reconnect if it becomes unresponsive.
