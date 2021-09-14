# Configure Active Directory settings<a name="join-domain-fsx"></a>

In this step, you configure your Amazon FSx File Gateway access settings in Storage Gateway to join a Microsoft Active Directory\.

------
#### [ New console ]

**To configure Active Directory settings**

1. In the Storage Gateway console, choose **Attach FSx file system**\.

1. On the **Confirm gateway** page, in the list of gateways, choose the Amazon FSx File Gateway that you want to use\. 

   If you don't have a gateway, you must create one\. Make sure your gateway can resolve the name of your Active Directory Domain Controller\. For information, see [Required prerequisites](Requirements.md#user-requirements)\.

1. Enter values for the **Active Directory settings**:
**Note**  
If your gateway is already joined to a domain, you don't need to join again\. Go to the next step\.
   +  For **Domain name**, enter the domain name of the Active Directory that you want to use\.
   + For **Domain user**, enter a user name for the Active Directory\.
   + For **Domain password**, enter the password for the domain user\.
**Note**  
Your account must be able to join a server to a domain\.
   + For **Organizational unit\- optional**, you can specify an organizational unit the Active Directory belongs to\.
   + Enter a value for **Domain controller\(s\) \- optional**\. 

1. Choose **Next** to open the **Attach FSx File system** page\.

------
#### [ Original console ]

**To configure Microsoft Active Directory access settings**

1. Choose **Gateways**, and on the Gateway page, select the check box next to the Amazon FSx file gateway that you want to join to a domain\.

   If you don't have a gateway, you must create one\. Make sure your gateway can resolve the name of your Active Directory Domain Controller\. For information, see [Required prerequisites](Requirements.md#user-requirements)\.

1. From **Actions**, choose **Edit SMB settings**\.

1. Choose the pencil icon in the upper\-right corner of the Active Directory settings section\.

1. Enter the values for **Domain name**, **Domain user**, and **Domain password**\. Optionally, you can specify an organizational unit, and domain controllers\. 

1. Choose **Save**, and then choose **Close**\.
**Note**  
If a file system is attached to a gateway, you can’t edit the domain information\. You need to detach the file system and then you can change the domain information\. The pencil icon for editing is not active, as seen following, if the gateway has one or more file systems attached\.

------

**Next step**

[Attach an Amazon FSx for Windows File Server file system](attach-fsxw-filesystem.md)