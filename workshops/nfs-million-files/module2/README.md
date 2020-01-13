# **AWS DataSync**

### Migrate millions of files using AWS DataSync

© 2020 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

# Module 2
## Configure the NFS server

Understanding how much data needs to be copied and how that data is organized is a key part of planning when using AWS DataSync. Three file systems have been created on the NFS server and are being pre-populated with randomly generated datasets.

In this module, you will browse the NFS server that was created in the previous module, verify that the datasets have been fully initialized, and then configure the NFS server with the proper export settings so it can be used by AWS DataSync.

## Module Steps

#### 1. Login to the NFS server

1. Go to the AWS Management console page in the **ON-PREMISES** region and click **Services** then select **EC2**.
2. Click on the list of instances and then select the NFS Server.  Click on the **Connect** button and follow the instructions to create an SSH connection to the NFS server.

#### 2. Verify the datasets have been completed

Before continuing, you will need to wait for the datasets on the NFS server to be initialized.  The server will download three archives and then extract the archives to the three file systems.  The process will take about 10-15 minutes to complete.  Once the datasets are initialized, you will see a file named **datasets_ready** in the **/home/ec2_user** directory.

#### 3. Browse the file systems

There are three 200 GiB EBS volumes attached to the NFS server.  Each volume has been formatted with an XFS file system and pre-populated with data from the previous step.

1. On the NFS server, run the following command to list the three file systems:

        [ec2-user@ ~]$ mount | grep /mnt

        /dev/nvme1n1 on /mnt/fs1 type xfs (rw,relatime,attr2,inode64,noquota)
        /dev/nvme2n1 on /mnt/fs2 type xfs (rw,relatime,attr2,inode64,noquota)
        /dev/nvme3n1 on /mnt/fs3 type xfs (rw,relatime,attr2,inode64,noquota)

2. The three file systems are mounted under the **/mnt** directory.  Run the following command to get the amount of data in each file system:

        [ec2-user@ ~]$ df -h | grep /mnt

        /dev/nvme1n1    200G   12G  189G   6% /mnt/fs1
        /dev/nvme2n1    200G   12G  189G   6% /mnt/fs2
        /dev/nvme3n1    200G   22G  179G  11% /mnt/fs3

  From the command output, you can see that fs1 and fs2 have **12 GiB** of data each while fs3 has **22 GiB** of data.

3. When using AWS DataSync it's important to know not only how much data will be transferred, but how many files will be transferred as well.  Run the following commands to determine the number of files and directories in each file system:

        [ec2-user@ ~]$ cd /mnt
        [ec2-user@ ~]$ ls -R fs1/ | wc -l
        [ec2-user@ ~]$ ls -R fs2/ | wc -l
        [ec2-user@ ~]$ ls -R fs3/ | wc -l

  You should see **505,151** files on fs1 and fs2, and **1,010,301** files on fs3.  If you take the amount of data on each file system and divide it by the number of files, you get an average file size of around **24 KiB**.  This means you are working primarily with small files and that a significant part of the I/O load on the NFS server will be metadata operations as DataSync processes millions of small files.

4. Go ahead and browse the directory structure a bit further.  Run the following commands to see how the files are organized:

        [ec2-user@ ~]$ ls fs1
        [ec2-user@ ~]$ ls fs1/d0001
        [ec2-user@ ~]$ ls -a fs1/d0001/dir0001

  These commands show there are 50 folders in fs1, each containing 20 sub-folders.  Each sub-folder contains 500 files along with three extra files: **.htaccess, index.html, manifest.lst.**  The other file systems are similar, except that fs3 has 100 top-level folders, rather than 50.  For this workshop, you will copy all files and directories to your S3 bucket except for .htaccess and index.html.

#### 4. Configure the NFS exports

You want to transfer data from all three file systems using DataSync.  To do so, you will need to create three NFS exports (aka file shares), one per file system.  On this version of Linux, you do this by modifying the **/etc/exports** file.

1. On the NFS server, **add the following three lines** to the /etc/exports file.  You will need to modify the file as root, so use the sudo command when starting your editor.

        /mnt/fs1 10.12.14.99(ro,no_root_squash) 10.12.14.16(ro,no_root_squash)
        /mnt/fs2 10.12.14.99(ro,no_root_squash) 10.12.14.16(ro,no_root_squash)
        /mnt/fs3 10.12.14.99(ro,no_root_squash) 10.12.14.16(ro,no_root_squash)

  Replace the IP addresses above with the **private** IP address assigned to the EC2 instances for each DataSync agent.
2. Run the following command to restart the NFS server and apply the export settings:

        [ec2-user@ ~]$ sudo systemctl restart nfs

## Validation Step

Run the following command to verify that the three file systems are being exported only to the DataSync agents:

    [ec2-user@ ~]$ showmount -e

    /mnt/fs3 10.12.14.16,10.12.14.99
    /mnt/fs2 10.12.14.16,10.12.14.99
    /mnt/fs1 10.12.14.16,10.12.14.99

## Module Summary

In this module you dove deep into the file systems on the NFS server to understand how much data and how many files will be transferred.  You also explored the directory layout which you will use in the next module to perform a test transfer.

To allow the DataSync agents to access the NFS server, you created NFS export entries for the three file systems. For this workshop, the agents have no need to write anything to the file system, so you configured each export to be read-only using the **'ro'** flag in the /etc/exports file.  Additionally, the DataSync agents will mount the NFS exports as the root user so you also specified the **'no_root_squash'** flag to make sure the agent has full access to all data on the file system.  Finally, you only exported the file systems to the DataSync agents, to lock down access.

In the next module, you will configure CloudWatch for logging by the DataSync service and you will activate your DataSync agents.

Go to [Module 3](/workshops/nfs-million-files/module3).
