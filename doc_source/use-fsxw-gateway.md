# Mount and use your file share<a name="use-fsxw-gateway"></a>

Before mounting your file share on the client, wait for the status of the Amazon FSx file system to change to **Available**\. After your file share is mounted, you can start using your Amazon FSx File Gateway \(FSx File\)\.

**Topics**
+ [Mount your SMB file share on your client](#mount-smb-fsxwfileshare)
+ [Test your FSx File](#TestFileFsxShare)

## Mount your SMB file share on your client<a name="mount-smb-fsxwfileshare"></a>

In this step, you mount your SMB file share and map to a drive that is accessible to your client\. The console's file gateway section shows the supported mount commands that you can use for SMB clients\. Following are some additional options to try\.

You can use several different methods for mounting SMB file shares, including the following:
+ The `net use` command – Doesn't persist across system reboots, unless you use the `/persistent:(yes:no)` switch\.
+ The `CmdKey` command line utility – Creates a persistent connection to a mounted SMB file share that remains after a reboot\.
+ A network drive mapped in File Explorer – Configures the mounted file share to reconnect at sign\-in and to require that you enter your network credentials\.
+ PowerShell script – Can be persistent, and can be either visible or invisible to the operating system while mounted\.

**Note**  
If you are a Microsoft Active Directory user, check with your administrator to ensure that you have access to the SMB file share before mounting the file share to your local system\.  
Amazon FSx File Gateway doesn't support SMB locking or SMB extended attributes\.

**To mount an SMB file share for Active Directory users using the net use command**

1. Make sure that you have access to the SMB file share before mounting the file share to your local system\.

1. For Microsoft Active Directory clients, enter the following command at the command prompt:

   **net use *\[WindowsDriveLetter\]*: \\\\*\[Gateway IP Address\]*\\*\[Name of the share on the FSx file system\]***

**To mount an SMB file share on Windows using CmdKey**

1. Press the Windows key and enter **cmd** to view the command prompt menu item\.

1. Open the context \(right\-click\) menu for **Command Prompt**, and choose **Run as administrator**\.

1. Enter the following command:

   **C:\\>cmdkey /add:*\[Gateway VM IP address\]* /user:*\[DomainName\]*\\*\[UserName\]* /pass:*\[Password\]***

**Note**  
When mounting file shares, you might need to remount your file share after rebooting your client\.

**To mount an SMB file share using Windows File Explorer**

1. Press the Windows key and enter **File Explorer** in the **Search Windows** box, or press **Win\+E**\.

1. In the navigation pane, choose **This PC**\.

1. On the **Computer** tab, choose **Map network drive**, and then choose **Map network drive** again, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/images/map-on-windows-explorer.png)  
  


1. In the **Map network drive** dialog box, choose a drive letter for **Drive**\.

1. For **Folder**, enter **\\\\*\[File Gateway IP\]*\\*\[SMB File Share Name\]***, or choose **Browse** to select your SMB file share from the dialog box\.

1. \(Optional\) Select **Reconnect at sign\-up** if you want your mount point to persist after reboots\.

1. \(Optional\) Select **Connect using different credentials** if you want a user to enter the Active Directory logon or guest account user password\.

1. Choose **Finish** to complete your mount point\.

## Test your FSx File<a name="TestFileFsxShare"></a>

 You can copy files and directories to your mapped drive\. The files automatically upload to your FSx for Windows File Server file system\.

**To upload files from your Windows client to Amazon FSx**

1. On your Windows client, navigate to the drive that you mounted your file share on\. The name of your drive is preceded by the name of your file system name\.

1. Copy files or a directory to the drive\.

**Note**  
File gateways don't support creating hard or symbolic links on a file share\.