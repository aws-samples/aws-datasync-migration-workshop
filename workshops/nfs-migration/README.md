# **AWS DataSync**

### NFS server migration using AWS DataSync and Storage Gateway

© 2019 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

## Workshop scenario

In your data center, you have an NFS server that is starting to age out.  Most of the data on the server is several years old and is only accessed for reading occasionally.  There are new files being written to the server but not very often.  To reduce your data center footprint and to free up resources, you would like to move the data on the NFS server into the cloud.  However, you cannot yet move your application servers that access the NFS data – those need to stay on-premises to minimize latency for your users.

After doing some research, you have realized that you can use AWS DataSync to migrate the data from your on-premises NFS server to Amazon S3, and you can use AWS Storage Gateway to provide NFS access on-premises to the data once it is in S3.

This workshop will walk you through this scenario, using CloudFormation templates to deploy resources and the AWS Management console to configure those resources accordingly.  As shown in the architecture diagram below, an NFS server, an Application server, a DataSync agent, and a File Gateway appliance will be deployed in an AWS region simulating the on-premises environment.  An S3 bucket will be created in an AWS region, simulating the AWS cloud region to which the NFS server&#39;s data will be migrated.

![](images/fullarch.png)

## Prerequisites

#### AWS Account

In order to complete this workshop, you will need an AWS account with rights to create AWS IAM roles, EC2 instances, AWS DataSync, AWS Storage Gateway and CloudFormation stacks in the AWS regions you select.

It will cost approximately 3 USD to run this workshop.  It is recommended that you follow the cleanup instructions once you have completed the workshop to remove all deployed resources and limit ongoing costs to your AWS account.

#### Client Software

- **Browser**  – It is recommended that you use the latest version of Chrome or Firefox for this workshop.

- **SSH Client**  - You will need an SSH client (i.e. Putty, etc.) to access EC2 instances.  Use the following links for instructions on how to install an SSH client, per your operating system:
  - [Connect from Windows using PuTTY](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-2-connect-to-instance.html#putty)
  - [Connect from Mac or Linux using an SSH client](https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/step-2-connect-to-instance.html#sshclient)


- **Key Pair**  – You will need a valid EC2 Key Pair in the AWS region you choose for your on-premises region. For more information on generating and downloading an EC2 Key Pair  see [creating a key pair using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair).

## Workshop Modules

This workshop consists of the following five modules:

- **Module 1**  - Deploy resources in the on-premises and in-cloud regions
- **Module 2** - Initial file copy to S3 using DataSync
- **Module 3**  - Access S3 bucket on-premises using File Gateway
- **Module 4**  - One last incremental copy before cutover
- **Module 5** - Cutover to File Gateway and shutdown the NFS server

To get started, go to [Module 1](../module1/README.md).
