
Managing Storage

Lab Overview: AWS provides multiple ways to manage data on Amazon Elastic Block Store (Amazon EBS) volumes. In this lab, I used AWS Command Line Interface (AWS CLI) to create snapshots of an EBS volume and configured a scheduler to run Python scripts to delete older snapshots.
In the challenge section of this lab, I was challenged to sync the contents of a directory on an EBS volume to an Amazon Simple Storage Service (Amazon S3) bucket using an Amazon S3 sync command.

Lab Environment: Your lab environment consists of a virtual private cloud (VPC) with a public subnet. Amazon Elastic Compute Cloud (Amazon EC2) instances named "Command Host" and "Processor" have already been created in this VPC for you as part of this lab.
The "Command Host" instance will be used to administer AWS resources including the "Processor" instance.
