# **AWS DataSync**

### Object store migration using AWS DataSync from Scality to Amazon S3

© 2019 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [owolabip@amazon.com](mailto:owolabip@amazon.com).

---

## Workshop scenario

In your data center, you have an Object store that is starting to age out. Over the years the data stored has continued to grow exponentially adding a significant management overhead. Your applications and service owners are making copies and replicas of the data stored in the object store to other locations such as AWS S3. To reduce your data center footprint and to free up resources, you would like to move the data on the object store into the cloud. Also, you want to ensure that the data is transfered in the native format and accessible to the applications and users already building solutions in the cloud.

After doing some research, you have realized that you can use [AWS DataSync](https://aws.amazon.com/datasync/) to migrate the data from your on-premises object store to Amazon S3. 

This workshop will walk you through this scenario, using CloudFormation templates to deploy resources and the AWS Management console to configure those resources accordingly.  As shown in the architecture diagram below, an object store (Scality - Zenko), an Application server, and a DataSync agent will be deployed in an AWS region simulating the on-premises environment.  An S3 bucket and an Application server will be created in an AWS region, simulating the AWS cloud region to which the object store data will be migrated.

![](images/fullarch.png)

## Topics covered

- Deploying resources using CloudFormation
- Registering and configuring DataSync agent
- Configure CloudWatch logging for DataSync
- Planning a migration of object using DataSync
- Using DataSync to perform incremental updates

## Prerequisites

#### AWS Account

In order to complete this workshop, you will need an AWS account with rights to create AWS IAM roles, EC2 instances, AWS DataSync and CloudFormation stacks in the AWS regions you select.

#### Software

- **Internet Browser**  – It is recommended that you use the latest version of Chrome or Firefox for this workshop.

## Cost

It will cost approximately **3.00 USD** to run this workshop.  It is recommended that you follow the cleanup instructions once you have completed the workshop to remove all deployed resources and limit ongoing costs to your AWS account.

## Related workshops

- [NFS server migration using AWS DataSync and AWS Storage Gateway](https://github.com/aws-samples/aws-datasync-migration-workshop/tree/master/workshops/nfs-migration)
- [Migrate millions of files using AWS DataSync](https://github.com/aws-samples/aws-datasync-migration-workshop/blob/master/workshops/nfs-million-files)
- [Migrate to FSx Windows File Server using AWS DataSync](https://github.com/aws-samples/aws-datasync-fsx-windows-migration)
- [Get hands-on with online data migration options to simplify & accelerate your journey to AWS](https://github.com/aws-samples/aws-online-data-migration-workshop)

## Workshop Modules

This workshop consists of the following five modules:

- [Module 1](module1/)  - Deploy resources in the on-premises and in-cloud regions
- [Module 2](module2/) - Initial copy from Scality to S3 using DataSync
- [Module 3](module3/)  - Securely access data on in-cloud region S3 bucket
- [Module 4](module4/)  - One last incremental copy before cutover
- [Module 5](module5/) - Cutover to AWS S3 and clean up resources

To get started, go to [Module 1](module1/).
