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


### Tasks followed for this Activity**

## Task 1: Modified a security group and observed the website

   **Step 1:**
   
    There should be two  inbound rules for the security group of the Cafe Web server
    1.  Type:HTTP, Port:80, Source:0.0.0.0/0
    2.  Type:SSH, Port:22, Source: WebServerIP   (Public IPv4 address value of the Cafe Web Server Instance)

   **Step 2:**
   
     Opened a new browser tab, and navigated to http://<WebServerIP>/cafe/ (substituted the <WebServerIP> value).
     Noticed that the website looks normal. For example, the photos were all appropriate for a bakery café.

## Task 2: Created a CloudTrail log and observed the hacked website

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

## Task 3: Used a variety of methods to analyze the CloudTrail logs, including the Linux grep utility and the AWS Command Line Interface (AWS CLI).

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

The results of the previous two command contained different details. Many describe and list actions were recorded, and they looked relatively harmless. However, I noticed that occasional update actions were also recorded. 

**Step 4:** Analyzed the logs using AWS CLI CloudTrail commands

Executed the following commands:
```
   aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=ConsoleLogin
```
The results indicated that there were no console login events or that the only user who logged into the console is the same user that I was logged into the console as.


Then I executed the following command to find any actions that were taken on security groups in the AWS account:
```
  aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup --output text
```
I thought thta something in this result set might contain some information that would help me discover what happened, but there were too many results for me to easily identify the issue.

I then narrowed the search results further so that I get only the results related to the security group that was used by the web server instance.
So, I used the following commands to find the security group ID that was used by the Café Web Server instance, and then used echo to show the result to the terminal:
```
   region=$(curl http://169.254.169.254/latest/dynamic/instance-identity/document|grep region | cut -d '"' -f4)
   sgId=$(aws ec2 describe-instances --filters "Name=tag:Name,Values='Cafe Web Server'" --query 'Reservations[*].Instances[*].SecurityGroups[*].     [GroupId]' --region $region --output text)

   echo $sgId
```
I noticed that a single security group ID was found.


Then , I used the security group ID that the previous command returned to further filter my  AWS CLI CloudTrail command results:
```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup --region $region --output    text | grep $sgId
```

I could have kept experimenting with different commands to filter the log results. However, I also wondered whether there was a better tool or solution for reading these logs.

---

## Task 4: Analyzed the CloudTrail logs by using Athena

The advantage of using Athena is that I could now run SQL queries over the log data.

**Step 1:** In the left panel of the Athena Query Editor, selected + beside the cloudtrail_logs_monitoring#### table.

`I noticed how each standard child element that existed in a CloudTrail log record in JSON format had a corresponding column name in this database. The useridentity database column was a struct type, because it contained more than a single name-value pair. Similarly, the resources database column was an array.`

**Step 2:** Set up query results location and then executed a simple query to get an idea of the data that is available in the logs.

On the menu bar at the upper right of the page, chose `Settings` followed by `Manage`.

-Set Location of query result to `s3://monitoring2345/results/` 

-Chose Save.

Selected the Editor table and typed the following SQL query into the Query 1 panel. Chose Run.

```
SELECT *
FROM cloudtrail_logs_monitoring2345
LIMIT 5
```

This query returned five rows of data.  The columns **useridentity, eventtime, eventsource, eventname, and requestparameters** contained the most valuable information to help me find the origin of the hack.
The **useridentity** column had many details that made it more difficult to read though. So I decided to return only the user name for that column.  

**Step 3:** Executed a new query that selected only the relevant columns. This time, I limited the results to 30 rows.

```
SELECT useridentity.userName, eventtime, eventsource, eventname, requestparameters
FROM cloudtrail_logs_monitoring####
LIMIT 30
```

![Image](https://github.com/gmonika-blip/my-aws-restart-journey/blob/dc96b142a59f967f724800f21a5d97eb56e75ed1/Labs/Security/Working%20with%20AWS%20CloudTrail/AthenaQuery.png)


**Challenge:** Identify the hacker

## Task 5: Analyzing the hack further and improving security

**Task 5.1:** Checked the OS users

In the terminal where I had an active SSH session to the web server instance, executed the following command to find out who was recently logged into this operating system (OS):

```
  sudo aureport --auth
```

There was evidence that a user other than ec2-user has logged in. Who is that `chaos-user`?

Ran the `who` command to figure out who is currently logged in:

```
  who
```

The user was still logged in! I wanted to get the user(s) off this instance right away!

Ran the following command to try to remove the chaos-user OS user:

```
  sudo userdel -r chaos-user
```

That didn't work because user was still logged in. However, the command did return the process number the user is onnected as.

I then ran the following command (after replacing ProcNum with the process number returned by the last command) to stop the process that had the active chaos-user login session:

```
  sudo kill -9 ProcNum
```

Executed the `who` command again to verify that the chaos-user OS user was no longer connected:

```
  who
```

**Now I (the ec2-user) was the only user connected.**

Run the following command to try to delete the `chaos-user` again:

```
  sudo userdel -r chaos-user
```

It succeeded this time. The `chaos-user` was deleted.

I ran the following command to verify no other suspicious OS users who could login:

```
   sudo cat /etc/passwd | grep -v nologin
```

The grep part of the command filtered out the OS users who do not have a login.

The root, sync, shutdown, and halt users are all standard OS users in Amazon Linux, so there were no other concerning user logins on this instance.

**Task 5.2:** Updated SSH security

1. Analyzed SSH settings on the instance.

`I removed the OS user who hacked into the instance, but how did they manage to connect to the EC2 instance by using SSH in the first place? The admin has been careful about who has access to the key pair file.` 

I checked the SSH settings on this instance using the following command:

```
  sudo ls -l /etc/ssh/sshd_config
```

I noticed the last modified timestamp for the file. This file was modified the same day! That was concerning.

I then executed the following command to edit the SSH configuration file in the VI editor:

```
   sudo vi /etc/ssh/sshd_config
```

Analyzed the details of this file. 
I noticed, on line 61, password authentication is enabled. This is definitely not a security best practice! That means that anyone who knows (or can correctly guess) the username and password combination of an OS user can remotely access this instance without using an SSH key pair. This setting needed to be corrected.

>Enter edit mode
>Moved my cursor (using the arrow up or down keys) to the PasswordAuthentication yes line and commented it out.
Next, moved my cursor to the #PasswordAuthentication line (line 63) by using the arrow keys and uncommented this line (remove the # character).

Choose the Esc key on your keyboard to exit edit mode.
Save the changes, and exit the VI editor using the :wq command.

Run the following command to restart the SSH service so that the changes go into effect:

sudo service sshd restart
Note: If running the command above interrupts your SSH connection, reestablish the SSH connection before continuing on to the next step.

Finally, in the EC2 console, return to the Web Server security group settings.

With the Web Server security group selected, go to the Inbound tab, and choose Edit.

Delete the inbound rule that allows port 22 access from 0.0.0.0/0 (the one the hacker created).

Save the change.

Nice work! You have kicked the hacker out of this instance and remove the login account that they used. You also updated the SSH settings so that only users who have the correct key pair and the same source IP address as you can connect to it.

   




 
