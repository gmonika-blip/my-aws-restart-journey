# Troubleshooting a Network Issue

**Objectives**

After completing this lab, the following skills were developed:

- Analyze a customer networking scenario  
- Troubleshoot AWS VPC connectivity issues  

---

## Scenario

I acted as a **Cloud Support Engineer at Amazon Web Services (AWS)**. A customer reported a networking issue within their AWS infrastructure.

### Customer Email

> Hello, Cloud Support!  
>
> When I created an Apache server through the command line, I could not ping it. I also received an error when entering the IP address in the browser. Could you please help figure out what is blocking my connection?  
>
> Thanks!  
>
> Ana  
> Contractor
<img src="images/CustomerDiagram.png" alt="App Screenshot" width="100%">


The customer’s environment included a **Virtual Private Cloud (VPC)** with:
- An Internet Gateway  
- A public subnet  
- An Amazon EC2 instance running an Apache web server  

---

## Accessing the AWS Management Console

- The lab was started using **Start Lab**  
- The system displayed **Lab status: ready** before proceeding  
- The AWS Console was opened via the **AWS link** provided  
- The session automatically logged in
- 
---

## Task 1: Use SSH to Connect to an Amazon Linux EC2 Instance

An SSH connection was established to an **Amazon Linux EC2 instance**. The steps varied depending on the operating system used (Windows or macOS/Linux).

### Windows Users (PuTTY)

- The **Details** panel was opened and credentials were displayed  
- The **PPK file** (`labsuser.ppk`) was downloaded  
- The **Public IP address** was recorded  
- **PuTTY** was installed and opened  
- A secure SSH session was configured using the provided guide  

### macOS/Linux Users

- The **PEM key** (`labsuser.pem`) was downloaded  
- The **Public IP address** was recorded  
- A terminal window was opened and the following steps were completed:

- The directory was changed to where the key file was downloaded:
  ```bash
  cd ~/Downloads

- Permissions for the key file were changed to be read-only:
  ```
  chmod 400 labsuser.pem

- The EC2 instance was accessed using SSH (replacing <public-ip> with the actual IP address):
  ```
  ssh -i labsuser.pem ec2-user@<public-ip>
  
- When prompted, yes was entered to confirm the connection. No password was required because authentication was performed using the key pair.

---

## Task 2: Install and Start HTTPD

1. The Apache HTTP server was verified and started using the following commands:
   ```
   </> Bash
   sudo systemctl status httpd.service
   sudo systemctl start httpd.service
   sudo systemctl status httpd.service
   ```
   **The service was confirmed as active (running).**

2. Tried to access the Test page using the **Public IP address** that was recorded in Task 1:
   ```
   http://<PUBLIC-IP-OF-INSTANCE>
   ```
   **The page did not load at this stage, indicating a potential networking issue.**

 
 ## Task 2: Investigate the customer's VPC configuration

Ana, the customer requesting assistance, cannot reach her Apache server even though it is active.
I checked each service within the VPC to confirm that each resource is configured correctly.

1. Subnets - Are the route tables associated to the correct subnets?
2. Route Tables - Do the route tables have the correct routes?
3. Internet Gateway - Is there an Internet Gateway and is it attached?
4. Security Groups and network ACLs - Are the correct rules configured?
  
   
# Troubleshooting a Network Issue



![AWS Architecture](./images/NF-06-architecture.png)

## Task 1: Install httpd

I logged to the EC2 istance using `SSH` from a terminal. Then I start the `httpd` server.

```bash
chiara@macbook-air:~/labs$ chmod 700 labsuser.pem 
chiara@macbook-air:~/labs$ ssh -i labsuser.pem ec2-user@44.251.232.254
The authenticity of host '44.251.232.254 (44.251.232.254)' can't be established.
ED25519 key fingerprint is SHA256:wr4OdMHh0JcbuNLj1y978d5XjYO6cbDeE4a6LUJs5Ro.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '44.251.232.254' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2
  ~~  \_#####\
  ~~     \###|       AL2 End of Life is 2026-06-30.
  ~~       \#/ ___
   ~~       V~' '->
    ~~~         /    A newer version of Amazon Linux is available!
      ~~._.   _/
         _/ _/       Amazon Linux 2023, GA and supported until 2028-03-15.
       _/m/'           https://aws.amazon.com/linux/amazon-linux-2023/

[ec2-user@ip-10-0-10-234 ~]$ 44.251.232.254
-bash: 44.251.232.254: command not found
[ec2-user@ip-10-0-10-234 ~]$ sudo systemctl status httpd.service
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd.service(8)
[ec2-user@ip-10-0-10-234 ~]$ sudo systemctl start httpd.service
[ec2-user@ip-10-0-10-234 ~]$ sudo systemctl status httpd.service
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2026-04-13 13:54:42 UTC; 5s ago
     Docs: man:httpd.service(8)
 Main PID: 2525 (httpd)
   Status: "Processing requests..."
   CGroup: /system.slice/httpd.service
           ├─2525 /usr/sbin/httpd -DFOREGROUND
           ├─2526 /usr/sbin/httpd -DFOREGROUND
           ├─2527 /usr/sbin/httpd -DFOREGROUND
           ├─2528 /usr/sbin/httpd -DFOREGROUND
           ├─2529 /usr/sbin/httpd -DFOREGROUND
           └─2530 /usr/sbin/httpd -DFOREGROUND

Apr 13 13:54:42 ip-10-0-10-234.us-west-2.compute.internal systemd[1]: Startin...
Apr 13 13:54:42 ip-10-0-10-234.us-west-2.compute.internal systemd[1]: Started...
Hint: Some lines were ellipsized, use -l to show in full.
[ec2-user@ip-10-0-10-234 ~]$ sudo systemctl status httpd.service
```

The httpd service is now running but it does not load on the public IP of the istance `http://44.251.232.254`.

## Task 2: Investigate the customer's VPC configuration

Ana, the customer requesting assistance, cannot reach her Apache server even though it is active.
I check each service within the VPC to confirm that each resource is configured correctly.

1. Subnets - Are the route tables associated to the correct subnets?
2. Route Tables - Do the route tables have the correct routes?
3. Internet Gateway - Is there an Internet Gateway and is it attached?
4. Security Groups and network ACLs - Are the correct rules configured?

I ping websites such as www.amazon.com.
```bash
[ec2-user@ip-10-0-10-234 ~]$ ping -c 4 www.amazon.com
PING cf.47cf2c8c9-frontier.amazon.com (3.163.26.68) 56(84) bytes of data.
64 bytes from server-3-163-26-68.hio52.r.cloudfront.net (3.163.26.68): icmp_seq=1 ttl=249 time=5.33 ms
64 bytes from server-3-163-26-68.hio52.r.cloudfront.net (3.163.26.68): icmp_seq=2 ttl=249 time=5.36 ms
64 bytes from server-3-163-26-68.hio52.r.cloudfront.net (3.163.26.68): icmp_seq=3 ttl=249 time=5.31 ms
64 bytes from server-3-163-26-68.hio52.r.cloudfront.net (3.163.26.68): icmp_seq=4 ttl=249 time=5.32 ms

--- cf.47cf2c8c9-frontier.amazon.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 5.314/5.334/5.362/0.075 ms
```
This confirm that I can get to the internet, so the internet gateway and route table are working.

Instead, the security group lacked an inbound rule allowing HTTP traffic (port 80) from the internet (0.0.0.0/0). I added this rule to 
the Linux instance SG security group and retested the Apache server using its public URL.

![HTTPD Server Working](./images/NF-06-httpd-running.png)


## Conclusion
- I analyzed the customer scenario.
- I investigated and fixed the issue.

## Additional resources
- [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
### Conclusion

This lab demonstrated how to effectively troubleshoot cloud networking issues using a combination of AWS VPC features, EC2 diagnostics, and VPC Flow Logs.

By the end of the exercise, connectivity to the web server was restored, and flow logs were successfully used to trace and analyze both allowed and denied traffic.

Overall, the lab reinforced practical skills in:
- Cloud network troubleshooting  
- Traffic flow analysis  
- Secure VPC architecture validation  
