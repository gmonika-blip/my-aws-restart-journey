**Scenario:**

Martha and Frank are concerned because their Cafe website was hacked. They want to discover who did it and to make sure that it does not happen again.

[Architecture Setup for this Activity](https://github.com/gmonika-blip/my-aws-restart-journey/blob/4cbcbacb13cd427ceebae6bcd988aace320af7b9/Labs/Security/Working%20with%20AWS%20CloudTrail/ArchitectureSetup-Lab187.png)

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

   




 
