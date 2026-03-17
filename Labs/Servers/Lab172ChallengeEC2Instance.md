
Lab : Amazon EC2 Instances Challenge

In this lab, I completed the following objectives:
-- Configure a virtual network
-- Place an Amazon Linux EC2 instance into this virtual network
-- Install a web server, and deploy and run a simple application on it.

To complete my objectives, I executed the following steps:
1. Used AWS Management console to launch the instance.
2. I selected the option of Amazon Linux Amazon Machine Image (AMI) and a T3 instance type with a size small.
3. Launch the instance in a new Virtual Private cloud (VPC) and a new subnet, and auto-assign the instance's public IPv4 address.
4. Use a General Purpose SSD (gp2) volume type for the root volume.
5. In the user data, install and start the httpd service as your web server. Give write permission to users to the web server's document root directory (/var/www/html).
6. Connect to the instance  by using Secure Shell (SSH).
7. Checked the Ec2 Instance's system log to see if the httpd service was successfully installed.

[EC2 instance's system log image](https://github.com/gmonika-blip/my-aws-restart-journey/blob/c6760596240e569b76f78093122170a5e78dfd41/Labs/Servers/Lab172Challenge-httpdServiceInstalled.png)
