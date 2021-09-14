# Managing your Amazon FSx File Gateway resources<a name="managing-gateway-file-fsx"></a>

The following sections provide information about how to manage your Amazon FSx File Gateway \(FSx File\) resources, including attaching and detaching Amazon FSx file systems, and configuring Microsoft Active Directory settings\.

**Topics**
+ [Attaching an Amazon FSx file system](#attach-file-system)
+ [Configuring Active Directory for your FSx File](#configuring-msft-ad)
+ [Configuring Active Directory settings](#editing-ad-settings)
+ [Editing FSx File settings](#edit-file-system-settings)
+ [Editing Amazon FSx for Windows File Server file system settings](#edit-file-system)
+ [Detaching an Amazon FSx file system](#detach-file-system)

## Attaching an Amazon FSx file system<a name="attach-file-system"></a>

You must have an FSx for Windows File Server file system before you can attach it to an FSx File\. If you don't have a file system, you must create one\. For instructions, see [Step 1: Create Your File System](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started-step1.html) in the *FSx for Windows File Server User Guide*\.

The next step is to activate an FSx File and configure your gateway to join an Active Directory domain\. For instructions, see [Configure Active Directory settings](join-domain-fsx.md)\.

**Note**  
When your gateway has joined a domain, you don't have to configure it to join the domain again\.

Each gateway can support up to five attached file systems\. For instructions on how to attach a file system, see [Attach an Amazon FSx for Windows File Server file system](attach-fsxw-filesystem.md)\.

## Configuring Active Directory for your FSx File<a name="configuring-msft-ad"></a>



To use FSx File, you are required to configure your gateway to join an Active Directory domain\. For instructions, see [Configure Active Directory settings](join-domain-fsx.md)\.

## Configuring Active Directory settings<a name="editing-ad-settings"></a>



After you configure your gateway to join an Active Directory domain, you can edit the Active Directory settings\.

**To edit Active Directory settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway whose Active Directory settings you want to edit\.

1. For **Actions**, choose **Edit SMB settings**, and then choose **Active Directory settings**\.

1. Provide the information requested in the Active Directory settings section, and then choose **Save changes**\.

## Editing FSx File settings<a name="edit-file-system-settings"></a>

After the gateway is activated, you can edit your gateway settings\.

**To edit your gateway settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway whose settings you want to edit\.

1. For **Actions**, choose **Edit gateway information**\.

1. For **Gateway name**, edit the name of your gateway that you selected\.

1. For **Gateway time zone**, choose a time zone\.

1. For **Gateway health log group**, choose one of the options to monitor your gateway using Amazon CloudWatch log groups\. 

   If you choose **Use an existing log group**, choose a log group from the **Existing log group list**, and then choose **Save changes**\.

## Editing Amazon FSx for Windows File Server file system settings<a name="edit-file-system"></a>

After creating an Amazon FSx for Windows File Server file system, you can edit the file system settings\.

------
#### [ New console ]

**To edit Amazon FSx file system settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **File system**, and choose the file system whose settings you want to edit\.

1. For **Actions**, choose **Edit file system settings**\.

1. In the file system settings section, verify the gateway, Amazon FSx location, and IP address information\.
**Note**  
You cannot edit a file system's IP address after it is attached to a gateway\. To change the IP address, you must detach and reattach the file system\.

1. In the **Audit logs** section, choose an option to use CloudWatch log groups to monitor access to Amazon FSx file systems\. You can use an existing log group\.

1. For **Automated cache refresh settings**, choose an option\. If you choose **Set refresh interval**, set the time in days, hours, and minutes to refresh the file system's cache using Time To Live \(TTL\)\. 

   TTL is the length of time since the last refresh\. When the directory is accessed after that length of time, the file gateway refreshes that directory's contents from the Amazon FSx file system\.
**Note**  
Valid refresh interval values are between 5 minutes and 30 days\.

    

1. In the **Service account settings \- optional** section, enter a user name and a **Password**\. These credentials are for a user that has the Backup Administrator role from the Active Directory service associated with your Amazon FSx file systems\.

1. Choose **Save changes**\.

------
#### [ Original console ]

**To edit Amazon FSx file system settings**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose** File system**, and then choose the file system whose settings you want to edit\.

1. For **Actions**, choose **Edit file system settings**\.

1. Verify the name of your file system\.

1. For **User**, enter a user name\. For **Password**, enter a password\.

1. For **Automated cache refresh from FSx after**, select the check box and set the time in days, hours, and minutes to refresh the file system's cache using Time To Live \(TTL\)\. 

   TTL is the length of time since the last refresh\. When the directory is accessed after that length of time, the file gateway refreshes that directory's contents from the Amazon FSx file system\. 

1. In the **Audit logs** section, choose an option to use CloudWatch log groups to monitor access to Amazon FSx file systems\. You can use an existing log group\.

1. Choose **Save**\.

------

## Detaching an Amazon FSx file system<a name="detach-file-system"></a>

Detaching a file system doesn't delete your data in FSx for Windows File Server\. Data that is written to the file shares on these the file systems before you delete the file system will still be uploaded to your FSx for Windows File Server\.

**To detach an Amazon FSx file system**

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the left navigation pane, choose **File system**, and then choose the file system that you want to detach\. You can delete multiple file systems\.

1. For **Actions**, choose **Detach file system**\.

1. Enter **detach** in the box to confirm, and choose **Detach**\.