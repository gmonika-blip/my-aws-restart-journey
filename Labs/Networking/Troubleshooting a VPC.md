# Troubleshooting a VPC – Lab Summary

### Lab Overview

In this lab, troubleshooting was performed on VPC configuration, and VPC Flow Logs were analyzed to identify and resolve networking issues in a cloud environment.

The environment consisted of two VPCs, EC2 instances, and supporting networking components. A CLI Host instance was used to run AWS CLI commands for troubleshooting and analysis.

The troubleshooting workflow followed a structured sequence:
1. Capturing network traffic using VPC Flow Logs  
2. Identifying connectivity issues affecting a web server instance  
3. Diagnosing and fixing routing, security group, and network ACL misconfigurations  
4. Validating access to the web server  
5. Analyzing flow logs to confirm rejected and accepted traffic patterns  

**Objectives**

By completing this lab, the following skills were developed:

- Creation and configuration of **VPC Flow Logs**
- Troubleshooting **VPC networking connectivity issues**
- Analysis of **network traffic logs** to identify access failures and patterns

---

### Scenario Summary

The lab simulated a real-world troubleshooting scenario where a web server hosted in a VPC was initially inaccessible due to misconfigured networking components. The objective was to restore connectivity while capturing and analyzing traffic data using VPC Flow Logs.

A CLI-based approach was used extensively to diagnose issues, including:
- Security group validation  
- Route table verification  
- Network ACL inspection  
- Log analysis using AWS CLI and Linux commands  

Email from the customer
>Hello, Cloud Support!

When I create an Apache server through the command line, I cannot ping it. I also get an error when I enter the IP address in the browser. Can you please help figure out what is blocking my connection?

Thanks!

>Ana
>Contractor

---

### Conclusion

This lab demonstrated how to effectively troubleshoot cloud networking issues using a combination of AWS VPC features, EC2 diagnostics, and VPC Flow Logs.

By the end of the exercise, connectivity to the web server was restored, and flow logs were successfully used to trace and analyze both allowed and denied traffic.

Overall, the lab reinforced practical skills in:
- Cloud network troubleshooting  
- Traffic flow analysis  
- Secure VPC architecture validation  
