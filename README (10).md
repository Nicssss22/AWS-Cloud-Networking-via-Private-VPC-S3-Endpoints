<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Endpoints

**Project Link:** [View Project](https://nextwork.ai/projects/749fab76-0a0b-5822-9b31-c7863050c9bd)

**Author:** agustinnico2302@gmail.com  
**Email:** agustinnico2302@gmail.com

---

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_09bcaa8a)

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC provides a private network for running and managing resources in AWS. You can adjust it to fit your needs by controlling security, routing traffic, and organizing resources into subnets.

### How I used Amazon VPC in this project

In today’s project, I used Amazon VPC to launch an EC2 instance in a public subnet, created access keys for the instance, and connected it to an S3 bucket. After that, I created a VPC endpoint so that my EC2 can connect with S3 using the endpoint so the traffic wouldn't have to travel via the public internet.

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was I still have to configure my routing table so that VPC knows where S3 traffic should be routed. Initially, I thought that VPC knows how to send those traffic to the endpoint since it is explicity specified and attached from the endpoint configuration. So it is the same setup as the VPC peering because there's a need to add route in the routing table even I already associated the peering connection to the VPC

### This project took me...

This project took me for about 2-3 hours since I got a hard time understand the mechanism behind S3 request such as the hostname resolutions and endpoints. Also, what took me time the most is the understand of the IAM Policies since I was trying to reserse engineer it in other way then predicting the result.

## In the first part of my project...

### Step 1 - Architecture set up

I will setup the foundation of this project by creating a VPC, launching an EC2 instance, and creating an S3 Bucket.

### Step 2 - Connect to EC2 instance

In this step, I will connect to my EC2 instance using the EC2 Instance Connect which would use the public internet accessing the instance. Remember that I configured the security group that allows inbound SSH traffic only, in which Instance Connect's protocol. 

### Step 3 - Set up access keys

In this step, I will be creating an access key so my EC2 instance can communicate with AWS services.

### Step 4 - Interact with S3 bucket

In this step, I will be using the newly created Access keys to enter it in my EC2 instance so it will finally be authenticated using my user account.

## Architecture set up

I started my project by launching VPC and an EC2 instance. I enable the S3 endpoint during the configurating of the VPC, instead, I disabled it so I can set it manually. In EC2, I created a security group that only allow inbound SSH traffic since I'll be using EC2 Instance Connect. 

I also set up the Amazon S3 by creating a bucket and adding two (2) objects inside of it.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d777)

## Access keys

### Credentials

To set up my EC2 instance to interact with my AWS environment, I created an Access Key using the IAM user administrator account. After that I selected the 'Command-line Interface' for use case as I'm using the access key for CLI of my EC2. I get back to my EC2 instance CLI then entered 'aws configure', asked me for access key ID, secret access key, region, and default format. I successfully authenticated my account and can received the expected output from aws s3 ls.

Access keys are the credentials applications and servers used to communicate with AWS services using the authenticated user account.

Secret access keys are like password when typing your credentials in any social media platform. It is paired with the Access Key ID to identity the identity which is commonly refered as the username. These pairs are the credentials to authenticate an application or a server to use an AWS service using an account.

### Best practice

Although this project uses access keys, a more secure option is to use AWS CLI v2’s `aws login` command to authenticate through a browser, much like signing in to the AWS Management Console. You can also use AWS CloudShell to run CLI commands directly from the console and interact with AWS services.

## Connecting to my S3 bucket

The command I ran was the 'aws s3 ls'. This command is used to list all my buckets in Amazon S3.

After typing the command, the terminal responded with the list of my buckets in Amazon S3. This indicated that the access keys I set up successfully authenticated my EC2 instance on my AWS user account.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d778)

## Connecting to my S3 bucket

I also tested the command 'aws s3 ls s3://nextwork-vpc-project-nicoagustins3vpc' which returned the list of objects in my S3 budket.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4334d779)

## Uploading objects to S3

To upload a new file to my bucket, I first ran the command 'sudo touch /tmp/test.txt' to create an empty file.  

Second is that I ran 'aws s3 cp /tmp/test.txt s3://nextwork-vpc-project-NicoAgustinS3' to copy this newly created file to the S3 bucket.

Lastly, I ran 'aws s3 ls s3://nextwork-vpc-project-nicoagustins3vpc' to verify if the operation is successful.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a2)

## In the second part of my project...

### Step 5 - Set up a Gateway

In this step, I will be setting up a VPC endpoint so that my instance can connect the my S3 bucket in a more secured way using the private network of AWS.

### Step 6 - Bucket policies

I'll be limiting the S3 bucket access to only traffic from the newly created gateway endpoint which forbids traffic coming from the public internet. 

### Step 7 - Update route tables

In this step, I will test the connectivity between EC2 instance and S3 bucket. I will also troubleshoot any connectivity issues that I may encounter.

### Step 8 - Validate endpoint conection

In this step, I will be testing again if the EC2 instance can finally communiate with my S3 bucket after troubleshooting connectivity issues.

## Setting up a Gateway

I set up an S3 Gateway, which is specifcally design for Amazon S3 and DynamoDB to securely connect them with the VPC. There is another endpoint called "Interface Endpoint" which offers more detailed settings.

### What are endpoints?

An endpoint is an AWS service that allows between the VPC and other AWS services that is not deployable inside the VPC. In this case, I created an endpoint so that VPC and by S3 bucket could have a connection within the AWS private network.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_09bcaa8a)

## Bucket policies

A bucket policy is a type of IAM policy that specifies who and what actions can be accessed in an S3 bucket.

My bucket policy will deny all the actions to everyone but only allow actions from the specified VPC gateway endpoint.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_7316a13d)

## Bucket policies

Right after saving my bucket policy, my S3 bucket page showed 'denied access' warnings. This was because I denied all the access to EVERYONE except the VPC endpoint.

I also had to update my route table because the VPC doesn't know how to send the traffic to the endpoint, instead is uses the public internet to access the S3 bucket which is be denied as the bucket policy only allow access from the VPC's endpoint. 

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_4ec7821f)

## Route table updates

To update my route table, I went back to my Endpoint then associated the route table from the public subnet of my VPC.

After updating my public subnet's route table, my terminal could now return the expected output, same as when the EC2 initially accessed the bucket via the internet.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_d116818e)

## Endpoint policies

An endpoint policy specifies which principals can use a VPC endpoint and what actions they can perform on resources through it.

I updated my endpoint's policy by changing the effect to Deny. I could see the effect of this right away, because when I tried to access the S3 bucket by the EC2, it denies the access due to the 'deny' effect of the VPC Endpoint Policy.

![Image](https://nextwork.ai/content_gray_heroic_hyena/uploads/749fab76-0a0b-5822-9b31-c7863050c9bd_3e1e79a3)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/749fab76-0a0b-5822-9b31-c7863050c9bd)*
