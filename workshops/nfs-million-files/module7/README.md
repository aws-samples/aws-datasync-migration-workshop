# **AWS DataSync**

### Migrate millions of files using AWS DataSync

© 2020 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

# Module 7
## Workshop clean-up

To make sure all resources are deleted after this workshop scenario make sure you execute the steps in the order outlined below (you do not need to wait for CloudFormation to finish deleting before moving to the next step):

1. Go to the **IN-CLOUD** region AWS management console and go to the **DataSync** service.
2. Select **Tasks** and delete the tasks you created during this workshop
3. Select **Locations** and delete the locations you created during this workshop
4. Select **Agents** and delete the agents you activated during this workshop.  Note that this **will not** delete the actual DataSync agent EC2 instances.  Those will get deleted when the associated on-premises CloudFormation stack is deleted.

5. Go to the CloudFormation page in the **ON-PREMISES** region and delete the stack named &quot;MillionFiles-OnPrem&quot;.

6. Delete all objects in the S3 bucket in the in-cloud region.  The bucket must be empty before it can be deleted by CloudFormation in the next step.  **NOTE:** due to the large number of files in the bucket, this step will take approximately 1.5 hours to complete.
7. Once the bucket is empty, go to the CloudFormation page in the **IN-CLOUD** region and delete the stack named &quot;MillionFiles-InCloud&quot;.


To make sure that all CloudFormation templates have been deleted correctly, confirm that all EC2 instances created in this workshop in the on-premises region are in the **terminated** state.
