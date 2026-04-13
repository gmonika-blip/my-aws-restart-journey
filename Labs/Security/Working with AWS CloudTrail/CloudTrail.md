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

**Task 1:** Modifying a security group and observing the website

   **Step 1:**
   
    There should be two  inbound rules for the security group of the Cafe Web server
    1.  Type:HTTP, Port:80, Source:0.0.0.0/0
    2.  Type:SSH, Port:22, Source: WebServerIP   (Public IPv4 address value of the Cafe Web Server Instance)

   **Step 2:**
   
     Open a new browser tab, and navigate to http://<WebServerIP>/cafe/ (substitute the <WebServerIP> value).
     Notice that the website looks normal. For example, the photos are all appropriate for a bakery café.

 
