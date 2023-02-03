--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Troubleshooting: File Gateway issues<a name="troubleshooting-file-gateway-issues"></a>

You can configure your File Gateway with an Amazon CloudWatch log group when you run VMware vSphere High Availability \(HA\)\. If you do, you receive notifications about your File Gateway's health status and about errors that the File Gateway encounters\. You can find information about these error and health notifications in CloudWatch Logs\.

In the following sections, you can find information that can help you understand the cause of each error and health notification and how to fix issues\.

**Topics**
+ [Error: FileMissing](#troubleshoot-logging-errors-filemissing)
+ [Error: FsxFileSystemAuthenticationFailure](#troubleshoot-logging-errors-fsxfilesystemauthenticationfailure)
+ [Error: FsxFileSystemConnectionFailure](#troubleshoot-logging-errors-fsxfilesystemconnectionfailure)
+ [Error: FsxFileSystemFull](#troubleshoot-logging-errors-fsxfilesystemfull)
+ [Error: GatewayClockOutOfSync](#troubleshoot-logging-errors-gatewayclockoutofsync)
+ [Error: InvalidFileState](#troubleshoot-logging-errors-invalidfilestate)
+ [Error: ObjectMissing](#troubleshoot-logging-errors-objectmissing)
+ [Notification: AvailabilityMonitorTest](#troubleshoot-availabilitymonitortest-notification)
+ [Notification: HardReboot](#troubleshoot-hardreboot-notification)
+ [Notification: HealthCheckFailure](#troubleshoot-healthcheckfailure-notification)
+ [Notification: Reboot](#troubleshoot-reboot-notification)
+ [Troubleshooting: Active Directory domain issues](#troubleshooting-ad-domain)
+ [Troubleshooting: Using CloudWatch metrics](#troubleshooting-with-cw-metrics)

## Error: FileMissing<a name="troubleshoot-logging-errors-filemissing"></a>

The `FileMissing` error is similar to the `ObjectMissing` error, and the steps to resolve it are identical\. You can get a `FileMissing` error when a writer other than the specified File Gateway deletes the specified file from the Amazon FSx\. Any subsequent uploads to Amazon FSx or retrievals from Amazon FSx for the object fail\.

**To resolve a FileMissing error**

1. Save the latest copy of the file to the local file system of your SMB client \(you need this file copy in step 3\)\.

1. Delete the file from the File Gateway using your SMB client\.

1. Copy the latest version of the file that you saved in step 1 Amazon FSx using your SMB client\. Do this through your File Gateway\.

## Error: FsxFileSystemAuthenticationFailure<a name="troubleshoot-logging-errors-fsxfilesystemauthenticationfailure"></a>

You can get an `FsxFileSystemAuthenticationFailure` error when the credentials provided while attaching the filesystem expired or, its privileges have been revoked\.

**To resolve an FsxFileSystemAuthenticationFailure error**

1. Ensure that the credentials provided at the time of attaching the Amazon FSx file system are still valid\.

1. Ensure that the user has all necessary permissions as described in [Attach an Amazon FSx for Windows File Server file system](https://docs.aws.amazon.com/filegateway/latest/filefsxw/attach-fsxw-filesystem.html)\.

## Error: FsxFileSystemConnectionFailure<a name="troubleshoot-logging-errors-fsxfilesystemconnectionfailure"></a>

You can get an `FsxFileSystemConnectionFailure` error when the Amazon FSx server is inaccessible from the gateway machine\.

**To resolve an FsxFileSystemConnectionFailure error**

1. Ensure that all the firewall and VPC rules are allowing the connection between the gateway machine and the Amazon FSx server\.

1. Ensure that the Amazon FSx server is running\.

## Error: FsxFileSystemFull<a name="troubleshoot-logging-errors-fsxfilesystemfull"></a>

You can get an `FsxFileSystemFull` error when there is not enough free disk space in the Amazon FSx file system\.

**To resolve an FsxFileSystemFull error**
+ Increase the storage space for the Amazon FSx file system\.

## Error: GatewayClockOutOfSync<a name="troubleshoot-logging-errors-gatewayclockoutofsync"></a>

You can get a `GatewayClockOutOfSync` error when the gateway detects a difference of 5 minutes or more between the local system time and the time reported by the AWS Storage Gateway servers\. Clock synchronization issues can negatively impact SMB connections\.

**To resolve a GatewayClockOutOfSync error**
+ Check the network configuration between the gateway and the NTP server\. For more information about synchronizing the gateway VM time and updating the NTP server configuration, see [Configuring a Network Time Protocol \(NTP\) server for your gateway](https://docs.aws.amazon.com/filegateway/latest/filefsxw/manage-on-premises-fgw.html#MaintenanceTimeSync-fgw)\.

## Error: InvalidFileState<a name="troubleshoot-logging-errors-invalidfilestate"></a>

You can get an `InvalidFileState` error when a writer other than the specified gateway modifies the specified file in the specified file share\. As a result, the state of the file on the gateway doesn’t match its state in Amazon FSx\. Any subsequent uploads or retrievals of the file from Amazon FSx could fail\.

**To resolve an InvalidFileState error**

1. Save the latest copy of the file to the local file system of your SMB client \(you need this file to copy in step 4\)\. If the version of the file in Amazon FSx is the latest, download that version\. You can do this by directly accessing the Amazon FSx share using any SMB client\.

1. Delete the file in Amazon FSx directly\.

1. Delete the file from the gateway using your SMB client\.

1. Using your SMB client, copy the latest version of the file that you saved in step 1, *through your File Gateway*,to Amazon FSx\.

## Error: ObjectMissing<a name="troubleshoot-logging-errors-objectmissing"></a>

You can get an `ObjectMissing` error when a writer other than the specified File Gateway deletes the specified file from the Amazon FSx\. Any subsequent uploads to Amazon FSx or retrievals from Amazon FSx for the object fail\.

**To resolve an ObjectMissing error**

1. Save the latest copy of the file to the local file system of your SMB client \(you need this file copy in step 3\)\.

1. Delete the file from the File Gateway using your SMB client\.

1. Copy the latest version of the file that you saved in step 1 Amazon FSx using your SMB client\. Do this through your File Gateway\.

## Notification: AvailabilityMonitorTest<a name="troubleshoot-availabilitymonitortest-notification"></a>

You get an `AvailabilityMonitorTest` notification when you [run a test](vmware-ha.md#vmware-ha-test-failover) of the [Availability and application monitoring](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html) system on gateways running on a VMware vSphere HA platform\.

## Notification: HardReboot<a name="troubleshoot-hardreboot-notification"></a>

You can get a `HardReboot` notification when the gateway VM is restarted unexpectedly\. Such a restart can be due to loss of power, a hardware failure, or another event\. For VMware gateways, a reset by vSphere High Availability Application Monitoring can trigger this event\.

When your gateway runs in such an environment, check for the presence of the `HealthCheckFailure` notification and consult the VMware events log for the VM\.

## Notification: HealthCheckFailure<a name="troubleshoot-healthcheckfailure-notification"></a>

For a gateway on VMware vSphere HA, you can get a `HealthCheckFailure` notification when a health check fails and a VM restart is requested\. This event also occurs during a test to monitor availability, indicated by an `AvailabilityMonitorTest` notification\. In this case, the `HealthCheckFailure` notification is expected\.

**Note**  
This notification is for VMware gateways only\.

If this event repeatedly occurs without an `AvailabilityMonitorTest` notification, check your VM infrastructure for issues \(storage, memory, and so on\)\. If you need additional assistance, contact AWS Support\.

## Notification: Reboot<a name="troubleshoot-reboot-notification"></a>

You can get a reboot notification when the gateway VM is restarted\. You can restart a gateway VM by using the VM Hypervisor Management console or the Storage Gateway console\. You can also restart by using the gateway software during the gateway's maintenance cycle\.

If the time of the reboot is within 10 minutes of the gateway's configured [maintenance start time](MaintenanceManagingUpdate-common.md), this reboot is probably a normal occurrence and not a sign of any problem\. If the reboot occurred significantly outside the maintenance window, check whether the gateway was restarted manually\.

## Troubleshooting: Active Directory domain issues<a name="troubleshooting-ad-domain"></a>

If you have trouble joining your gateway to your Active Directory domain, do the following:
+ Verify that the gateway is not attempting to use a read\-only domain controller \(RODC\) to join the domain\.
+ Verify that the gateway is configured to use the correct DNS servers\.

  For example, if you are trying to join an Amazon EC2 gateway instance to an AWS\-managed Active Directory, verify that the DHCP option set for your EC2 VPC specifies the AWS\-managed Active Directory DNS servers\.

  DNS servers that you configure through the VPC DHCP options set are provided to the all EC2 instances in the VPC\. If you want to specify a DNS server for an individual gateway, you can do so using that gateway's EC2 local console\.

  For on\-premises gateways, you specify a DNS server using the VM local console\.
+ Verify gateway network connectivity by running the following commands from the command prompt in the gateway's local console\. Replace the highlighted variables with the actual domain name and IP addresses from your deployment\.

  ```
  dig -d ExampleDomainName
  ncport -d ExampleDomainControllerIPAddress -p 445
  ncport -d ExampleDomainControllerIPAddress -p 389
  ```
+ Verify that the gateway joins the correct Organizational Unit \(OU\)\.

  Joining a domain creates an Active Directory computer account in the default computers container \(which is not an OU\), using the gateway's **Gateway ID** as the account name \(for example, SGW\-1234ADE\)\.

  If your Active Directory environment has a designated OU for new computer objects, you must specify that OU when joining the domain\.

  If you encounter access denied errors when attempting to join the designated OU, check with your Active Directory domain administrator\. The administrator may need to pre\-stage the gateway's computer account before it can join the domain\. For more information, see [How can I troubleshoot issues with joining my Storage Gateway file gateway to a domain for Microsoft Active Directory authentication?](https://aws.amazon.com/premiumsupport/knowledge-center/storage-gateway-domain-join-error/)\.
+ Verify that your gateway's hostname is resolvable in DNS by running the following command from the command prompt in the gateway's local console\. Replace the highlighted variable with the actual hostname for your gateway\.

  ```
  dig -d ExampleHostName -r A
  ```

  If you configured a custom hostname for your gateway, you must manually add a DNS A\-record that points to its IP address\.
+ Verify that network latency between the gateway and the domain controller is reasonably low\. The query to join a domain can time out if the gateway does not receive a response from the domain controller within 20 seconds\.

  If you join the gateway to the domain using the [JoinDomain](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_JoinDomain.html) CLI command, you can can add the `--timeout-in-seconds` flag to increase the timeout to a maximum of 3,600 seconds\.
+ Verify that the Active Directory user you are using to join the gateway to the domain has the privileges required to do so\.

**Note**  
When a join domain operation fails, the gateway returns a JoinDomainFailure error to assist you help troubleshoot the issue\. For more information, see [Error: JoinDomainFailure](https://docs.aws.amazon.com/filegateway/latest/filefsxw/troubleshooting-file-gateway-issues.html#error-join-domain-failure)\.

## Troubleshooting: Using CloudWatch metrics<a name="troubleshooting-with-cw-metrics"></a>

You can find information following about actions to address issues using Amazon CloudWatch metrics with Storage Gateway\.

**Topics**
+ [Your gateway reacts slowly when browsing directories](#slow-gateway)
+ [Your gateway isn't responding](#gateway-not-responding)
+ [You do not see files in your Amazon FSx file system](#files-missing-fsx)
+ [Your gateway is slow transferring data to Amazon FSx](#slow-data-transfer-to-fsx)
+ [Your gateway backup job fails or there are errors when writing to your gateway](#backup-job-fails)

### Your gateway reacts slowly when browsing directories<a name="slow-gateway"></a>

If your File Gateway reacts slowly when you run the ls command or browse directories, check the `IndexFetch` and `IndexEviction` CloudWatch metrics:
+ If the `IndexFetch` metric is greater than 0 when you run an `ls` command or browse directories, your File Gateway started without information on the contents of the directory affected and had to access Amazon S3\. Subsequent efforts to list the contents of that directory should go faster\.
+ If the `IndexEviction` metric is greater than 0, it means that your File Gateway has reached the limit of what it can manage in its cache at that time\. In this case, your File Gateway has to free some storage space from the least recently accessed directory to list a new directory\. If this occurs frequently and there is a performance impact, contact AWS Support\. 

  Discuss with AWS Support the contents of the related Amazon FSx file system and recommendations to improve performance based on your use case\.

### Your gateway isn't responding<a name="gateway-not-responding"></a>

If your File Gateway isn't responding, do the following:
+  If there was a recent reboot or software update, then check the `IOWaitPercent` metric\. This metric shows the percentage of time that the CPU is idle when there is an outstanding disk I/O request\. In some cases, this might be high \(10 or greater\) and might have risen after the server was rebooted or updated\. In these cases, then your File Gateway might be bottlenecked by a slow root disk as it rebuilds the index cache to RAM\. You can address this issue by using a faster physical disk for the root disk\.
+ If the `MemUsedBytes` metric is at or nearly the same as the `MemTotalBytes` metric, then your File Gateway is running out of available RAM\. Make sure that your File Gateway has at least the minimum required RAM\. If it already does, consider adding more RAM to your File Gateway based on your workload and use case\. 

  If the file share is SMB, the issue might also be due to the number of SMB clients connected to the file share\. To see the number of clients connected at any given time, check the `SMBV(1/2/3)Sessions` metric\. If there are many clients connected, you might need to add more RAM to your File Gateway\.

### You do not see files in your Amazon FSx file system<a name="files-missing-fsx"></a>

If you notice that files on the gateway are not reflected in the Amazon FSx file system, check the `FilesFailingUpload` metric\. If the metric reports that some files are failing upload, check your health notifications\. When files fail to upload, the gateway generates a health notification containing more details on the issue\.

### Your gateway is slow transferring data to Amazon FSx<a name="slow-data-transfer-to-fsx"></a>

If your File Gateway is slow transferring data to Amazon S3, do the following:
+ If the `CachePercentDirty` metric is 80 or greater, your File Gateway is writing data faster to disk than it can upload the data to Amazon S3\. Consider increasing the bandwidth for upload from your File Gateway, adding one or more cache disks, or slowing down client writes\.
+ If the `CachePercentDirty` metric is low, check the `IoWaitPercent` metric\. If `IoWaitPercent` is greater than 10, your File Gateway might be bottlenecked by the speed of the local cache disk\. We recommend local solid state drive \(SSD\) disks for your cache, preferably NVM Express \(NVMe\)\. If such disks aren't available, try using multiple cache disks from separate physical disks for a performance improvement\.

### Your gateway backup job fails or there are errors when writing to your gateway<a name="backup-job-fails"></a>

If your File Gateway backup job fails or there are errors when writing to your File Gateway, do the following:
+ If the `CachePercentDirty` metric is 90 percent or greater, your File Gateway can't accept new writes to disk because there is not enough available space on the cache disk\. To see how fast your File Gateway is uploading to Amazon FSx or Amazon S3, view the `CloudBytesUploaded` metric\. Compare that metric with the `WriteBytes` metric, which shows how fast the client is writing files to your File Gateway\. If your File Gateway is writing faster than it can upload to Amazon FSx or Amazon S3, add more cache disks to cover the size of the backup job at a minimum\. Or, increase the upload bandwidth\.
+ If a backup job fails but the `CachePercentDirty` metric is less than 80 percent, your File Gateway might be hitting a client\-side session timeout\. For SMB, you can increase this timeout using the PowerShell command `Set-SmbClientConfiguration -SessionTimeout 300`\. Running this command sets the timeout to 300 seconds\.

  For NFS, make sure that the client is mounted using a hard mount instead of a soft mount\.