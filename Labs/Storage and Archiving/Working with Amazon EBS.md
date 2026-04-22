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
