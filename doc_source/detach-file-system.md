--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Detaching an Amazon FSx file system<a name="detach-file-system"></a>

Detaching a file system doesn't delete your data in FSx for Windows File Server\. Data that is written to these the file systems before you detach them will still be uploaded to your FSx for Windows File Server\.

**To detach an Amazon FSx file system**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **FSx file systems**, then select one or more file systems to detach\.

1. For **Actions**, choose **Detach file system**\. The confirmation dialog box appears\.

1. Verify that you want to detach the specified file systems, then type the word *detach* in the confirmation box and choose **Detach**\. 