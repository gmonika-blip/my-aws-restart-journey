# Activity : Working with AWS CloudTrail


**Scenario:**

Martha and Frank are concerned because their Cafe website was hacked. They want to discover who did it and to make sure that it does not happen again.

<br>
<img width="808" height="470" alt="ArchitectureSetup-Lab187" src="https://github.com/user-attachments/assets/51da8f2c-a326-45cc-b16b-e96c7af98b68" />

<br>
<br>

The activity starts with an Amazon Elastic Compute Cloud (Amazon EC2) instance named Café Web Server, which runs a web application that hosts the Café website.

**Activity Objectives**

-- Configure a CloudTrail trail

-- Analyze CloudTrail logs by using various methods to discover relevant information

-- Import CloudTrail log data into Athena

-- Run queries in Athena to filter CloudTrail log entries

-- Resolve security concerns within the AWS account and on an EC2 Linux instance


**Tasks followed for this Activity**

**Task 1:** Modified a security group and observed the website

   **Step 1:**
   
    There should be two  inbound rules for the security group of the Cafe Web server
    1.  Type:HTTP, Port:80, Source:0.0.0.0/0
    2.  Type:SSH, Port:22, Source: WebServerIP   (Public IPv4 address value of the Cafe Web Server Instance)

   **Step 2:**
   
     Opened a new browser tab, and navigated to http://<WebServerIP>/cafe/ (substituted the <WebServerIP> value).
     Noticed that the website looks normal. For example, the photos were all appropriate for a bakery café.

**Task 2:** Created a CloudTrail log and observed the hacked website

In this task, I created a CloudTrail trail in my AWS account. I noticed that soon after creating the trail, the Café website was hacked.

**Step 1:**

  Created a CloudTrail log within AWS Console using the following configuration:
  
      Trail name - monitor
      
      Selected - Create a new S3 bucket
      
      Trail log bucket and folder - monitoring2345
      
      AWS KMS alias - mg-KMS

 Reviewed and Verified the trail created on the Trails page.

**Step 2:**

   Observed the hacked website by refreshing the Cafe website page.
   Noticed that the website has been hacked. Who put that image there? The image certainly did not look correct.
   
   It is good that CloudTral was enabled before this incident. CloudTrail can give us valuable information about        whatusers have been doing in your account.

   I looked into the inbound rules for the security group for the Cafe Web Server Instance.
   In addition to the two inbound rules created by me earlier, there was one more inbound rule created by someone   that allowed Secure   Shell (SSH) access from anywhere (0.0.0.0/0).

   Who added this security rule?  I searched the CloudTrail logs to find out.

**Task 3:** Used a variety of methods to analyze the CloudTrail logs, including the Linux grep utility and the AWS Command Line Interface (AWS CLI).

**Step 1:** Connected to the Café Web Server host EC2 instance via SSH using a private key

      - For Mac/Linux Users:  
           -  Downloaded and saved the labsuser.pem file
           -  Changed the permissions on the key to be read only
           -  Used SSH command to connect
           
                  ssh → starts the SSH client
                 -i labsuser.pem → specifies your private key file
                 ec2-user → default username for many Amazon Linux instances
                 <public-ip> → replace with your EC2 instance’s public IP address
```
            chmod 400 labsuser.pem
            ssh -i labsuser.pem ec2-user@<public-ip>
```
     When prompted, typed yes to allow a first connection to this remote SSH server.
     Since I was using a key pair for authentication, I was not prompted for a password.

**Step 2:** Downloaded and extracted the CloudTrail logs

After terminal is connected via SSH to the Café Web Server EC2 instance, I created a local directory (ctraillogs) on the web server to download the CloudTrail log files to. After changing the directory to the new directory, I listed the buckets to recall the bucket name. Then, I copied/downloaded all the CloudTrail logs in the bucket into the newly created directory.

```
    mkdir ctraillogs
    cd ctraillogs
    aws s3 ls
    aws s3 cp s3://<monitoring####>/ . --recursive

If the command is successful, you should see that a few log files are downloaded.

Important: If there was no output in the command line when you ran the last command, it likely means that not enough time has passed since you created the CloudWatch trail. CloudWatch posts logs to Amazon Simple Storage Service (Amazon S3) every 5 minutes. You might need to wait and try running the command again. Do not proceed to the next step until you have downloaded at least one log file.

Use the cd and ls commands repeatedly (or enter cd and then press Tab multiple times) as necessary to change the directory to the subdirectory where the logs were downloaded. When you run ls, all of the downloaded log files should display. They will be located in an AWSLogs/<account-num>/CloudTrail/<Region>/<yyyy>/<mm>/<dd> subdirectory.

Notice that the log files end in json.gz, which indicates that they are compressed as GNU zip files.

Run the following command to extract the logs:

gunzip *.gz
Run ls again. Notice that all files are now extracted.


**Task 4:** Analyzed the CloudTrail logs by using Athena

**Challenge:** Identify the hacker

**Task 5:** Analyzing the hack further and improving security

   




 
