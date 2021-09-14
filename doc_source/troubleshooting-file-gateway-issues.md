# Troubleshooting file gateway issues<a name="troubleshooting-file-gateway-issues"></a>

You can configure your file gateway with an Amazon CloudWatch log group when you run VMware vSphere High Availability \(HA\)\. If you do, you receive notifications about your file gateway's health status and about errors that the file gateway encounters\. You can find information about these error and health notifications in CloudWatch Logs\.

In the following sections, you can find information that can help you understand the cause of each error and health notification and how to fix issues\.

**Topics**
+ [Error: ObjectMissing](#troubleshoot-logging-errors-objectmissing)
+ [Notification: Reboot](#troubleshoot-reboot-notification)
+ [Notification: HardReboot](#troubleshoot-hardreboot-notification)
+ [Notification: HealthCheckFailure](#troubleshoot-healthcheckfailure-notification)
+ [Notification: AvailabilityMonitorTest](#troubleshoot-availabilitymonitortest-notification)
+ [Error: RoleTrustRelationshipInvalid](#misconfig-trust)
+ [Troubleshooting with CloudWatch metrics](#troubleshooting-with-cw-metrics)

## Error: ObjectMissing<a name="troubleshoot-logging-errors-objectmissing"></a>

You can get an `ObjectMissing` error when a writer other than the specified file gateway deletes the specified file from the Amazon FSx\. Any subsequent uploads to Amazon FSx or retrievals from Amazon FSx for the object fail\.

**To resolve an ObjectMissing error**

1. Save the latest copy of the file to the local file system of your SMB client \(you need this file copy in step 3\)\.

1. Delete the file from the file gateway using your SMB client\.

1. Copy the latest version of the file that you saved in step 1 Amazon FSx using your SMB client\. Do this through your file gateway\.

## Notification: Reboot<a name="troubleshoot-reboot-notification"></a>

You can get a reboot notification when the gateway VM is restarted\. You can restart a gateway VM by using the VM Hypervisor Management console or the Storage Gateway console\. You can also restart by using the gateway software during the gateway's maintenance cycle\.

If the time of the reboot is within 10 minutes of the gateway's configured [maintenance start time](MaintenanceManagingUpdate-common.md), this reboot is probably a normal occurrence and not a sign of any problem\. If the reboot occurred significantly outside the maintenance window, check whether the gateway was restarted manually\.

## Notification: HardReboot<a name="troubleshoot-hardreboot-notification"></a>

You can get a `HardReboot` notification when the gateway VM is restarted unexpectedly\. Such a restart can be due to loss of power, a hardware failure, or another event\. For VMware gateways, a reset by vSphere High Availability Application Monitoring can trigger this event\.

When your gateway runs in such an environment, check for the presence of the `HealthCheckFailure` notification and consult the VMware events log for the VM\.

## Notification: HealthCheckFailure<a name="troubleshoot-healthcheckfailure-notification"></a>

For a gateway on VMware vSphere HA, you can get a `HealthCheckFailure` notification when a health check fails and a VM restart is requested\. This event also occurs during a test to monitor availability, indicated by an `AvailabilityMonitorTest` notification\. In this case, the `HealthCheckFailure` notification is expected\.

**Note**  
This notification is for VMware gateways only\.

If this event repeatedly occurs without an `AvailabilityMonitorTest` notification, check your VM infrastructure for issues \(storage, memory, and so on\)\. If you need additional assistance, contact AWS Support\.

## Notification: AvailabilityMonitorTest<a name="troubleshoot-availabilitymonitortest-notification"></a>

You get an `AvailabilityMonitorTest` notification when you [run a test](Performance.md#vmware-ha-test-failover) of the [Availability and application monitoring](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_StartAvailabilityMonitorTest.html) system on gateways running on a VMware vSphere HA platform\.

## Error: RoleTrustRelationshipInvalid<a name="misconfig-trust"></a>

You get this error when the IAM role for a file share has a misconfigured IAM trust relationship \(that is, the IAM role does not trust the Storage Gateway principal named `storagegateway.amazonaws.com`\)\. As a result, the file gateway would not be able to get the credentials to run any operations on the S3 bucket that backs the file share\.

**To resolve an RoleTrustRelationshipInvalid error**
+ Use the IAM console or IAM API to include `storagegateway.amazonaws.com` as a principal that is trusted by your file share's IAMrole\. For information about IAM role, see [Tutorial: delegate access across AWS accounts using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html)\.

## Troubleshooting with CloudWatch metrics<a name="troubleshooting-with-cw-metrics"></a>

You can find information following about actions to address issues in using Amazon CloudWatch metrics with Storage Gateway\.

**Topics**
+ [Your gateway reacts slowly when browsing directories](#slow-gateway)
+ [Your gateway isn't responding](#gateway-not-responding)
+ [Your gateway is slow transferring data to Amazon FSx](#slow-data-transfer-to-fsx)
+ [Your gateway backup job fails or there are errors when writing to your gateway](#backup-job-fails)

### Your gateway reacts slowly when browsing directories<a name="slow-gateway"></a>

If your file gateway reacts slowly when you run the ls command or browse directories, check the `IndexFetch` and `IndexEviction` CloudWatch metrics:
+ If the `IndexFetch` metric is greater than 0 when you run an `ls` command or browse directories, your file gateway started without information on the contents of the directory affected and had to access Amazon S3\. Subsequent efforts to list the contents of that directory should go faster\.
+ If the `IndexEviction` metric is greater than 0, it means that your file gateway has reached the limit of what it can manage in its cache at that time\. In this case, your file gateway has to free some storage space from the least recently accessed directory to list a new directory\. If this occurs frequently and there is a performance impact, contact AWS Support\. 

  Discuss with AWS Support the contents of the related Amazon FSx file system and recommendations to improve performance based on your use case\.

### Your gateway isn't responding<a name="gateway-not-responding"></a>

If your file gateway isn't responding, do the following:
+  If there was a recent reboot or software update, then check the `IOWaitPercent` metric\. This metric shows the percentage of time that the CPU is idle when there is an outstanding disk I/O request\. In some cases, this might be high \(10 or greater\) and might have risen after the server was rebooted or updated\. In these cases, then your file gateway might be bottlenecked by a slow root disk as it rebuilds the index cache to RAM\. You can address this issue by using a faster physical disk for the root disk\.
+ If the `MemUsedBytes` metric is at or nearly the same as the `MemTotalBytes` metric, then your file gateway is running out of available RAM\. Make sure that your file gateway has at least the minimum required RAM\. If it already does, consider adding more RAM to your file gateway based on your workload and use case\. 

  If the file share is SMB, the issue might also be due to the number of SMB clients connected to the file share\. To see the number of clients connected at any given time, check the `SMBV(1/2/3)Sessions` metric\. If there are many clients connected, you might need to add more RAM to your file gateway\.

### Your gateway is slow transferring data to Amazon FSx<a name="slow-data-transfer-to-fsx"></a>

If your file gateway is slow transferring data to Amazon S3, do the following:
+ If the `CachePercentDirty` metric is 80 or greater, your file gateway is writing data faster to disk than it can upload the data to Amazon S3\. Consider increasing the bandwidth for upload from your file gateway, adding one or more cache disks, or slowing down client writes\.
+ If the `CachePercentDirty` metric is low, check the `IoWaitPercent` metric\. If `IoWaitPercent` is greater than 10, your file gateway might be bottlenecked by the speed of the local cache disk\. We recommend local solid state drive \(SSD\) disks for your cache, preferably NVM Express \(NVMe\)\. If such disks aren't available, try using multiple cache disks from separate physical disks for a performance improvement\.

### Your gateway backup job fails or there are errors when writing to your gateway<a name="backup-job-fails"></a>

If your file gateway backup job fails or there are errors when writing to your file gateway, do the following:
+ If the `CachePercentDirty` metric is 90 percent or greater, your file gateway can't accept new writes to disk because there is not enough available space on the cache disk\. To see how fast your file gateway is uploading to Amazon FSx or Amazon S3, view the `CloudBytesUploaded` metric\. Compare that metric with the `WriteBytes` metric, which shows how fast the client is writing files to your file gateway\. If your file gateway is writing faster than it can upload to Amazon FSx or Amazon S3, add more cache disks to cover the size of the backup job at a minimum\. Or, increase the upload bandwidth\.
+ If a backup job fails but the `CachePercentDirty` metric is less than 80 percent, your file gateway might be hitting a client\-side session timeout\. For SMB, you can increase this timeout using the PowerShell command `Set-SmbClientConfiguration -SessionTimeout 300`\. Running this command sets the timeout to 300 seconds\.

  For NFS, make sure that the client is mounted using a hard mount instead of a soft mount\.