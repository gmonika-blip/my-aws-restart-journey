
Managing Storage

Lab Overview: AWS provides multiple ways to manage data on Amazon Elastic Block Store (Amazon EBS) volumes. In this lab, I used AWS Command Line Interface (AWS CLI) to create snapshots of an EBS volume and configured a scheduler to run Python scripts to delete older snapshots.
In the challenge section of this lab, I was challenged to sync the contents of a directory on an EBS volume to an Amazon Simple Storage Service (Amazon S3) bucket using an Amazon S3 sync command.

Lab Environment: Consists of a virtual private cloud (VPC) with a public subnet. Amazon Elastic Compute Cloud (Amazon EC2) instances named "Command Host" and "Processor" have already been created in this VPC for me as part of this lab.
The "Command Host" instance will be used to administer AWS resources including the "Processor" instance.

[Lab Environment Architecture Diagram](https://github.com/gmonika-blip/my-aws-restart-journey/blob/ba1854fcfcd86c4817de39f4e94fd448d834eefd/Labs/Storage%20and%20Archiving/Lab183Enironment.png)
