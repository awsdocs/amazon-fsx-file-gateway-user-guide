--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Editing Amazon FSx for Windows File Server file system settings<a name="edit-file-system"></a>

After creating an Amazon FSx for Windows File Server file system, you can edit the file system settings\.

**To edit Amazon FSx file system settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **File system**, and choose the file system whose settings you want to edit\.

1. For **Actions**, choose **Edit file system settings**\.

1. In the file system settings section, verify the gateway, Amazon FSx location, and IP address information\.
**Note**  
You cannot edit a file system's IP address after it is attached to a gateway\. To change the IP address, you must detach and reattach the file system\.

1. In the **Audit logs** section, choose an option to use CloudWatch log groups to monitor access to Amazon FSx file systems\. You can use an existing log group\.

1. For **Automated cache refresh settings**, choose an option\. If you choose **Set refresh interval**, set the time in days, hours, and minutes to refresh the file system's cache using Time To Live \(TTL\)\. 

   TTL is the length of time since the last refresh\. When the directory is accessed after that length of time, the File Gateway refreshes that directory's contents from the Amazon FSx file system\.
**Note**  
Valid refresh interval values are between 5 minutes and 30 days\.

    

1. In the **Service account settings \- optional** section, enter a user name and a **Password**\. These credentials are for a user that has the Backup Administrator role from the Active Directory service associated with your Amazon FSx file systems\.

1. Choose **Save changes**\.