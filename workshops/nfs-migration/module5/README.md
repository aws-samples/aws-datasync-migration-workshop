# **AWS DataSync**

### NFS server migration using AWS DataSync and Storage Gateway

© 2019 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

# Module 5
## Cutover to File Gateway and shutdown the NFS server

With all of the data in the S3 bucket, you are now ready to shut down your NFS server and move exclusively to using the File Gateway.  In this module, you will unmount the NFS server and clean up your DataSync resources.  You will then write some test files through File Gateway, verifying they end up in the S3 bucket.

![](../images/mod5arch.png)

## Module Steps

#### 1. Unmount the NFS server

1. From the CLI for the Application server, run the following command to unmount the NFS server:

        $ sudo umount /mnt/data

#### 2. Clean up DataSync resources

You&#39;re done with DataSync so you can go ahead and clean up resources.

1. Go to the **in-cloud** region AWS management console and go to the **DataSync** service.

2. Select **Tasks** and delete the task you created previously
3. Select **Locations** and delete the locations you created previously
4. Select **Agents** and delete the agent you activated previously.  Note that this **will not** delete the actual DataSync agent EC2 instance.  That will get deleted later, when the CloudFormation stack is deleted.

## Validation Step

From the CLI for the Application server, run the following command to create another new file in the S3 bucket through the File Gateway:

    sudo cp /mnt/fgw/images/00002.jpg /mnt/fgw/new-image2.jpg

Go back to the in-cloud region management console and go to **S3**.  Select the **data-migration-workshop** bucket.  You should see the new-image2.jpg file in the bucket.

![](../images/mod5s31.png)

Your Application server has completed cutover!  You can now read all of the files that used to be on the NFS server using the File Gateway share.  And any new files written to the share will automatically be uploaded to the S3 bucket.  You can now shutdown and decommission your NFS server!

One of the benefits of using File Gateway is that it stores files as complete, wholly accessible objects in S3.  With your data is in S3, you can now use services such as Amazon Athena, Amazon SageMaker, Amazon EMR, and many other AWS services to gain even greater value and insight from your data.

## Workshop Cleanup

To make sure all resources are deleted after this workshop scenario make sure you execute the steps in the order outlined below (you do not need to wait for CloudFormation to finish deleting before moving to the next step):

1. Unmount the File Gateway NFS share on the Application server by running the following command:

        sudo umount /mnt/fgw

2. Delete the File Gateway **NFS file share** in the in-cloud region
3. Delete the File Gateway in the in-cloud region named **DataMigrationGateway**.  Note this will not delete the gateway EC2 instance.  The instance will get deleted when the CloudFormation in the on-premises region is deleted.
4. Delete all objects in the **data-migration-workshop** S3 bucket in the in-cloud region.  The bucket must be empty before it can be deleted by CloudFormation in the next step.

5. Go to the CloudFormation page in the in-cloud region and delete the stack named &quot;DataMigrationWorkshop-inCloudResources&quot;
6. Go to the CloudFormation page in the on-premises region and delete the stack named &quot;DataMigrationWorkshop-onPremResources&quot;

To make sure that all CloudFormation templates have been deleted correctly, confirm that all EC2 instances created in this workshop in the on-premises region are in the **terminated** state.
