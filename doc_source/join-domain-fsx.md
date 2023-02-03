--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Configure Active Directory settings<a name="join-domain-fsx"></a>

In this step, you configure your Amazon FSx File Gateway access settings in Storage Gateway to join a Microsoft Active Directory\.

**To configure Active Directory settings**

1. In the Storage Gateway console, choose **FSx file systems** from the navigation menu\.

1. Choose **Attach FSx file system**\.

1. On the **Confirm gateway** page, choose the gateway you want to join to your Active Directory domain from the drop\-down menu\.

   If you don't have a gateway, you must create one\. Make sure your gateway can resolve the name of your Active Directory Domain Controller\. For information, see [Required prerequisites](Requirements.md#user-requirements)\.

1. Enter values for the **Active Directory settings**:
**Note**  
If your gateway is already joined to a domain, you don't need to join again\. Go to the next step\.
   +  For **Domain name**, enter the domain name of the Active Directory that you want to use\.
   + For **Domain user**, enter the user name of the Active Directory user that you want to use to join the gateway to the domain\. This user must have the necessary permissions\.
   + For **Domain password**, enter the password for the user\.
   + For **Organizational unit\- optional**, you can specify an organizational unit the Active Directory belongs to\.
**Note**  
If you leave this field blank, joining a domain creates an Active Directory computer account in the default computers container \(which is not an OU\), using the gateway's **Gateway ID** as the account name \(for example, SGW\-1234ADE\)\.  
If your Active Directory environment requires that you pre\-stage accounts to facilitate the join domain process, you will need to create this account ahead of time\.  
If your Active Directory environment has a designated OU for new computer objects, you must specify that OU when joining the domain\.
   + Enter a value for **Domain controller\(s\) \- optional**\. 

1. Choose **Next** to open the **Attach FSx File system** page\.

**Next step**

[Attach an Amazon FSx for Windows File Server file system](attach-fsxw-filesystem.md)