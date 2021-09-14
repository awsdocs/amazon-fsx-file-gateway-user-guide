# Attach an Amazon FSx for Windows File Server file system<a name="attach-fsxw-filesystem"></a>

The next step is to attach an Amazon FSx file system to the gateway\. When you attach an Amazon FSx file system, all the file shares on the file system are made available to Amazon FSx File Gateway \(FSx File\) for you to mount\.

**Note**  
The following limitations apply when writing to an Amazon FSx file system from Amazon FSx File Gateway:  
Your Amazon FSx file system and your FSx File must be owned by the same AWS account and located in the same AWS Region\.
Each gateway can support up to five attached file systems\. When you're attaching a file system, the Storage Gateway console notifies you if the selected gateway is at capacity\. In that case, you must choose a different gateway or detach a file system before you can attach another one\.
FSx File supports soft storage quotas \(which warn you when users surpass their data limits\), but does not support hard quotas \(which enforce data limits by denying write access\)\. Soft quotas are supported for all users except the Amazon FSx admin user\. For more information about setting up storage quotas, see [Storage quotas](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/managing-user-quotas.html) in the Amazon FSx User Guide\.

------
#### [ New console ]

**To attach an Amazon FSx file system**

1. In the Storage Gateway console, on the **FSx file systems** > **Attach FSx file system** page, complete the following fields in the **FSx file system settings** section:
   + For **FSx file system name**, choose the file system that you want to attach from the dropdown list\.
   + For **Local Endpoint IP address**, enter the gateway IP address that clients will use to browse file shares on the FSx file system\.
**Note**  
If you plan to attach only one file system to your gateway, you can leave this field blank to make shares on the file system available on all of the gateway's IP addresses\. If you plan to attach multiple file systems, you must specify an IP address for each of them\.
If you attach a file system without an IP address and need to attach another file system later, you must detach the first file system and reattach it with an IP address\.
For Amazon EC2 gateways, you can specify the private IP address of the EC2 instance, unless it is already in use by a different file system, in which case you must add a new private address to the gateway, then restart it\. For more information, see [Multiple IP addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/MultipleIP.html) in the *Amazon EC2 User Guide*\.
For on\-premises gateways, you can specify the IP address of the primary network interface \(static or DHCP\), unless it is already in use by a different file system, in which case you must provide a different IP address from the same subnet as the primary interface, which will be made available as a virtual IP\. Do not use an IP address assigned to any network interface other than the primary\.

1. In the **Service account settings** section, provide the user name and password that is associated with the Amazon FSx file system\.
**Note**  
This user must have the backup administrator role from the Active Directory service that is associated with your Amazon FSx file systems\.  
To ensure sufficient permissions to files, folders, and file metadata, we recommend that you make this user a member of the file system administrators group\. If you are using AWS Directory Service for Microsoft Active Directory with Amazon FSx for Windows File Server, the user must be a member of the AWS Delegated FSx Administrators group\. If you are using a self\-managed Active Directory with Amazon FSx for Windows File Server, the user must be a member of one of two groups: the domain admins or the custom delegated file system administrators group you specified for the file system administration when you created your file system\.

1. In the **Audit logs** section, choose **Existing log groups**, and choose the log that you want to use to monitor access to your Amazon FSx file system\. You can create a new one\. If you don't want to monitor your system, choose** Disable logging**\.

1. For **Automated cache refresh setting**, if you want your cache to refresh automatically, choose **Set refresh interval** and specify an interval between 5 minutes and 30 days\.

1. \(Optional\) In the **Tags** section, choose **Add new tag** to add one or more keys and a value for tagging your settings\.

1. Choose **Next** and review the settings\. To change your settings, you can choose **Edit** in each section\.

1. When you are done, choose **Finish**\.

------
#### [ Original console ]

**To attach an Amazon FSx file system**

1. From the top menus, choose **Attach FSx file system**\. You are redirected to the **Configure settings** page\.

1. For **Amazon FSx File System name**, choose the Amazon FSx file system that you want to attach\. If you don't have an Amazon FSx file system, create one\.

1. For **Gateway**, choose the FSx File that you want to associate the file system with\.

1. For **User**, enter the user name that you use to access the file system\.

1. For **Password**, enter the password for the file system\.

1. For **Audit logs**, choose one of the options to use a log group\. You can use an existing log group\.

1. For **Automated cache refresh from FSx**, select the check box and set the time in days, hours, and minutes to refresh the file system's cache using Time To Live \(TTL\)\. TTL is the length of time since the last refresh\. When the directory is accessed after that length of time, the file gateway refreshes that directory's contents from the Amazon FSx file system\. 

1. In the **Add tags** section, you can add one or more keys and a value for your settings\.

1. Choose **Next**, review your choices, and choose **Attach file system\.**\.

------

**Next step**

[Mount and use your file share](use-fsxw-gateway.md)