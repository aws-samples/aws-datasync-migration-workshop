# **AWS DataSync**

### NFS server migration using AWS DataSync and AWS Storage Gateway

© 2019 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

# Module 3
## Access S3 bucket on-premises using File Gateway

You now have the files from the NFS server copied to your S3 bucket.  In this module, you will configure the File Gateway in the on-premises region to connect to your S3 bucket and provide access to the files in the bucket through an NFS share.  You will mount the File Gateway share on the Application server to validate access to the files.

![](../images/fullarch.png)

## Module Steps

#### 1. Activate the File Gateway

Just as you activated the DataSync agent in the previous module, you need to perform a similar step for the File Gateway, activating it in the **in-cloud** region.  Follow the steps below to activate the gateway.

1. Go to the AWS Management console page in the **in-cloud** region and click  **Services**  then select  **Storage Gateway.**

2. If no gateways exist, click the **Get started** button, otherwise click the **Create gateway** button.
3. Select the **File gateway** type and click **Next.**
4. Select **Amazon EC2** as the host platform, then click **Next**.
5. Select the **Public** endpoint type, then click **Next**.
6. Enter the **Public IP address** of the File Gateway instance that was created in the first module using CloudFormation.  Click **Connect to gateway**.
7. Name the gateway &quot;DataMigrationGateway&quot; then click **Activate gateway**.
8. The gateway will be activated and then it will spend a minute or so preparing the local disk devices.  Allocate the **300 GiB /dev/sdc** device to **Cache.**  This is the local disk on the gateway that will be used to cache frequently accessed files.
9. Click **Configure logging.**
10. Leave the setting at _Disable Logging_ then click **Save and continue.**
11. From the main Storage Gateway page, you will see your gateway listed.

  ![](../images/mod3fgw1.png)

#### 2. Create an NFS share

1. Click on the **Create file share** button
2. For the **Amazon S3 bucket name** , enter the name of the S3 bucket that DataSync copied the files to.  You can find the bucket name in the outputs of the CloudFormation stack in the **in-cloud** region.
3. Select **NFS** as the access method and make sure your gateway from the previous step is selected.
4. Click **Next**.
5. Keep the default settings, then click **Next**
6. Under the **Allowed clients** section, click **Edit** and change &quot;0.0.0.0/0&quot; to the **Private IP Address** of the Application server, followed by "/32".  This will only allow the Application server to access the NFS file share on the gateway.  Click the **Close** button.
7. Under the **Mount options** section, change the **Squash level** to &quot;No root squash&quot;.  Click the **Close** button.
8. Click **Create file share**.
9. Select the check box next to the new file share and note the mount instructions.

  ![](../images/mod3fgw2.png)

#### 3. Mount the NFS share on the Application server

1. Return to the CLI for the Application server and run the following command to create a new mount point for the File Gateway share:

        $ sudo mkdir /mnt/fgw

1. Copy the Linux mount command from the Storage Gateway file share page and replace &quot;[MountPath]&quot; with &quot;/mnt/fgw&quot;.   **You must run the command as sudo.**
2. You should now have two NFS mount points on your Application server: one for the on-premises NFS server (mounted at /mnt/data) and one for the File Gateway (mounted at /mnt/fgw).

  ![](../images/mod3cli1.png)

## Validation Step

Run the following command to verify that the same set of files exist on both NFS shares.

    $ diff -qr /mnt/data /mnt/fgw

You should see only one extra file in /mnt/fgw: .aws-datasync-metadata.  This file was created by DataSync in the S3 bucket when the task was executed.  All other files are the same, indicating that our data was fully transferred by DataSync without errors.

## Module Summary

In this module you successfully activated the File Gateway and created an NFS file share on the gateway.  You then mounted the share on the Application server and verified that the files from the on-premises NFS server were copied correctly to the S3 bucket.

Remember that our ultimate goal in this workshop is to shut off the on-premises NFS server and free up storage resources.  In a production environment, this would typically involve a &quot;cutover point&quot;, where there is momentary downtime as the Application server changes over to the new storage, which in this workshop is the File Gateway NFS share.  However, there are usually new files being created while a migration occurs, or shortly after, requiring another incremental file copy before cutover.

In the next module, you&#39;ll do one more incremental copy before the final cutover to the File Gateway share.

Go to [Module 4](module4/).
