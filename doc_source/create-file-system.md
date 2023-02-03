--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Step 1: Create an Amazon FSx for Windows File Server file system<a name="create-file-system"></a>

To create an Amazon FSx File Gateway in AWS Storage Gateway, the first step is to create an Amazon FSx for Windows File Server file system\. If you've already created an Amazon FSx file system, go to the next step, [Step 2: \(Optional\) Create an Amazon VPC endpoint](create-vpc-endpoint-fsx.md)\.

**Note**  
The following limitations apply when writing to an Amazon FSx file system from an FSx File Gateway:  
Your Amazon FSx file system and your FSx File Gateway must be owned by the same AWS account and located in the same AWS Region\.
Each gateway can support five attached file systems\. When attaching a file system, the Storage Gateway console notifies you if the selected gateway is at capacity\. In that case, you must choose a different gateway or detach a file system before you can attach another one\.
FSx File Gateway supports soft storage quotas \(issuing warnings when users surpass their data limits\), but does not support hard quotas \(enforcing data limits by denying write access\)\. Soft quotas are supported for all users except the Amazon FSx admin user\. For more information about setting up storage quotas, see [Storage quotas](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/managing-user-quotas.html) in the *Amazon FSx for Windows File Server User Guide*\.

**To create an FSx for Windows File Server file system**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/fsx/home/](https://console.aws.amazon.com/fsx/home/), and choose the Region that you want to create your gateway in\.

1. Follow the instructions in [Getting Started with Amazon FSx](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started.html) in the *Amazon FSx for Windows File Server User Guide*\.