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

After terminal is connected via SSH to the Café Web Server EC2 instance, I created a local directory (ctraillogs) on the web server to download the CloudTrail log files to. After changing the directory to the new directory, I listed the buckets to recall the bucket name (monitoring2345). Then, I copied/downloaded all the CloudTrail logs in the bucket into the newly created directory.  I used the following commands:

```
    mkdir ctraillogs
    cd ctraillogs
    aws s3 ls
    aws s3 cp s3://<monitoring####>/ . --recursive
```


The command was successful, I could see that a few log files were downloaded.  I noticed that the log files end in json.gz, which indicated that they are compressed as GNU zip files.

Used the following command to extract the logs:

```
   gunzip *.gz
```

I executed the ls command again and noticed that all files were now extracted.

**Step 3:** Analyzed the logs by using **grep**

The grep utility in Linux is used to search for text patterns inside files or command output. It’s one of the most useful command-line tools for filtering and finding information quickly.

I executed the following command:
```
   for i in $(ls); do echo $i && cat $i | python -m json.tool | grep sourceIPAddress ; done
```
This command creates a for loop that includes the names of the files in the current directory.
During each iteration of the for loop, it echoes the file name and then prints the contents of the file in JSON format.
Only the lines of JSON that contain the sourceIPAddress tag are printed.
I noticed that there were several log entries in the trail where the sourceIPAddress was the Café Web Server instance.


Then, I also executed a similarly structured command but where the command returns the eventName of every captured event:
```
   for i in $(ls); do echo $i && cat $i | python -m json.tool | grep eventName ; done
```

**Task 4:** Analyzed the CloudTrail logs by using Athena

**Challenge:** Identify the hacker

**Task 5:** Analyzing the hack further and improving security

   




 
