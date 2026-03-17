Lab : Creating a Website on Amazon S3

In this lab, I practiced using AWs Command Line Interface (AWS CLI) commands from an Amazon Elastic Compute (Amazon EC2) instance to:

-- Create an Amazon Simple Storage Service (Amazon S3) bucket.

-- Create a new AWS Identity and Access Management (IAM) user that has full access to the Amazon S3 service.

-- Upload files to Amazon S3 to host a simple website for the Café & Bakery.

-- Create a batch file that can be used to update the static website when you change any of the website files locally.

[Click Architecture image](https://github.com/gmonika-blip/my-aws-restart-journey/blob/310538ea29948e223db591af9716d3f24eb2a33f/Labs/Servers/Lab170ArchitectureImage.png)

My steps for this lab:

1. I accessed the AWS Management Console
2. Connected to an Amazon Linux EC2 instance using SSM.
3. Configured the AWS CLI
      Configured the AWS Access Key ID, AWS Secret Access Key, Default region name and the default output format.
4.  Created an S3 bucket using the AWS CLI. I named this bucket gmonika1.
5.  Created a new IAM user that has full access to Amazon S3.
       When I logged in as this IAM user, I could see the bucket, gmonika1, that I had created using AWS CLI. [Click Bucket Created image](https://github.com/gmonika-blip/my-aws-restart-journey/blob/2ac5fae7def9b1b7b6aa9abe316824b0a28c8ccf/Labs/Servers/Lab170CreateBucket.png)

6. On the AWS console, adjusted the S# bucket permissions.
7. Uploaded the files to Amazon S3 using the AWS CLI. [Click Screenshot](https://github.com/gmonika-blip/my-aws-restart-journey/blob/c478ed566d758fc07db948b7519cb108b2f878c6/Labs/Servers/Lab170UploadFilestoS3usingCLI.png)
   
    After choosing gmonika1 bucket on the AWS management Console, I clicked on the Bucket website endpoint URL.  This         displayed my static website.  


   
