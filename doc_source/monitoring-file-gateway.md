# Monitoring your file gateway<a name="monitoring-file-gateway"></a>

You can monitor your file gateway and associated resources in Storage Gateway by using Amazon CloudWatch metrics and file share audit logs\. You can also use CloudWatch Events to get notified when your file operations are done\. For information about file gateway type metrics, see [Monitoring your file gateway](#monitoring-file-gateway)\.

**Topics**
+ [Getting file gateway health logs with CloudWatch log groups](#cw-log-groups)
+ [Using Amazon CloudWatch metrics](#using-CloudWatch-metrics)
+ [Understanding file system metrics](#monitoring-file-gateway-resources)
+ [Understanding file gateway audit logs](#audit-logs)

## Getting file gateway health logs with CloudWatch log groups<a name="cw-log-groups"></a>

You can use Amazon CloudWatch Logs to get information about the health of your file gateway and related resources\. You can use the logs to monitor your gateway for errors that it encounters\. In addition, you can use Amazon CloudWatch subscription filters to automate processing of the log information in real time\. For more information, see [Real\-time Processing of Log Data with Subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) in the *Amazon CloudWatch User Guide\.*

For example, you can configure a CloudWatch log group to monitor your gateway and get notified when your file gateway fails to upload files to an Amazon FSx file system\. You can configure the group either when you are activating the gateway or after your gateway is activated and up and running\. For information about how to configure a CloudWatch log group when activating a gateway, see [Configure Amazon CloudWatch logging](configure-loging-file.md)\. For general information about CloudWatch log groups, see [ Working with Log Groups and Log Streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch User Guide\.*

The following is an example of an error reported by a file gateway\. 

In the preceding gateway health log, these items specify the given information:
+ `source: share-E1A2B34C` indicates the file share that encountered this error\.
+ `"type": "InaccessibleStorageClass"` indicates the type of error that occurred\. In this case, this error was encountered when the gateway was trying to upload the specified object to Amazon S3 or read from Amazon S3\. However, in this case, the object has transitioned to Amazon S3 Glacier\. The value of `"type"` can be any error that the file gateway encounters\. For a list of possible errors, see [Troubleshooting file gateway issues](troubleshooting-file-gateway-issues.md)\.
+  `"operation": "S3Upload"` indicates that this error occurred when the gateway was trying to upload this object to S3\.
+ `"key": "myFolder/myFile.text"` indicates the object that caused the failure\.
+ `gateway": "sgw-B1D123D4` indicates the file gateway that encountered this error\.
+ `"timestamp": "1565740862516"` indicates the time that the error occurred\.

For information about how to troubleshoot and fix these types of errors, see [Troubleshooting file gateway issues](troubleshooting-file-gateway-issues.md)\.

### Configuring a CloudWatch log group after your gateway is activated<a name="creat-cwlogroup"></a>

The following procedure shows you how to configure a CloudWatch Log Group after your gateway is activated\.

------
#### [ New console ]

**To configure a CloudWatch log group to work with your file gateway**

1. Sign in to the AWS Management Console and open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**, and then choose the gateway that you want to configure the CloudWatch log group for\.

1. For **Actions**, choose **Edit gateway information**\. Or, on the **Details** tab, under **Health logs** and **Not Enabled**, choose **Configure log group** to open the **Edit *CustomerGatewayName*** dialog box\.

1. For **Gateway health log group**, choose one of the following:
   + **Disable logging** if you don't want to monitor your gateway using CloudWatch log groups\.
   + **Create a new log group** to create a new CloudWatch log group\.
   + **Use an existing log group** to use a CloudWatch log group that already exists\.

     Choose a log group from the **Existing log group list**\.

1. Choose **Save changes**\.

1. To see the health logs for your gateway, do the following:

   1. In the navigation pane, choose **Gateways**, and then choose the gateway that you configured the CloudWatch log group for\.

   1. Choose the **Details** tab, and under **Health logs**, choose **CloudWatch Logs**\. The **Log group details** page opens in the CloudWatch console\.

------
#### [ Original console ]

**To configure a CloudWatch Log Group to work with your file gateway**

1. Sign in to the AWS Management Console and open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. Choose **Gateways**, and then choose the gateway that you want to configure the CloudWatch log group for\. 

1. For **Actions**, choose **Edit gateway information**\. Or, in the **Details** tab, next to **Logging**, under **Not Enabled**, choose **Configure log group** to open the **Edit gateway information** dialog box\.

1. For **Gateway log group**, choose **Use an existing log group**, and then choose the log group that you want to use\. 

   If you don't have a log group, choose **Create a new log group** to create one\. You are directed to the CloudWatch Logs console where you can create the log group\. If you create a new log group, choose the refresh button to view the new log group in the drop\-down list\.

1. When you are done, choose **Save**\.

1. To see the logs for your gateway, choose the gateway, and then choose the **Details** tab\.

------

For information about how to troubleshoot errors, see [Troubleshooting file gateway issues](troubleshooting-file-gateway-issues.md)\.

## Using Amazon CloudWatch metrics<a name="using-CloudWatch-metrics"></a>

You can get monitoring data for your file gateway by using either the AWS Management Console or the CloudWatch API\. The console displays a series of graphs based on the raw data from the CloudWatch API\. The CloudWatch API can also be used through one of the [AWS SDKs](http://aws.amazon.com/tools) or [Amazon CloudWatch API](http://aws.amazon.com/cloudwatch) tools\. Depending on your needs, you might prefer to use either the graphs displayed in the console or retrieved from the API\.

Regardless of which method you use to work with metrics, you must specify the following information:
+ The metric dimension to work with\. A *dimension* is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Storage Gateway are `GatewayId` and `GatewayName`\. In the CloudWatch console, you can use the `Gateway Metrics` view to select gateway\-specific dimensions\. For more information about dimensions, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch User Guide*\.
+ The metric name, such as `ReadBytes`\.

The following table summarizes the types of Storage Gateway metric data that are available to you\.


| Amazon CloudWatch namespace | Dimension | Description | 
| --- | --- | --- | 
| AWS/StorageGateway |  GatewayId, GatewayName  |  These dimensions filter for metric data that describes aspects of the gateway\. You can identify a file gateway to work with by specifying both the `GatewayId` and the `GatewayName` dimensions\. Throughput and latency data of a gateway are based on all the file shares in the gateway\. Data is available automatically in 5\-minute periods at no charge\.   | 

Working with gateway and file metrics is similar to working with other service metrics\. You can find a discussion of some of the most common metrics tasks in the CloudWatch documentation listed following:
+ [Viewing available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)
+ [Getting statistics for a metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/getting-metric-statistics.html)
+ [Creating CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)

## Understanding file system metrics<a name="monitoring-file-gateway-resources"></a>

You can find information following about the Storage Gateway metrics that cover file shares\. Each file share has a set of metrics associated with it\. Some file share\-specific metrics have the same name as certain gateway\-specific metrics\. These metrics represent the same kinds of measurements, but are scoped to the file share instead\. 

Always specify whether you want to work with either a gateway or a file share metric before working with a metric\. Specifically, when working with file share metrics, you must specify the `File share ID` that identifies the file share for which you are interested in viewing metrics\. For more information, see [Using Amazon CloudWatch metrics](#using-CloudWatch-metrics)\.

The following table describes the Storage Gateway metrics that you can use to get information about your file shares\.


| Metric | Description | 
| --- | --- | 
| CacheHitPercent |  Percent of application read operations from the file shares that are served from cache\. The sample is taken at the end of the reporting period\. When there are no application read operations from the file share, this metric reports 100 percent\.  Units: Percent  | 
| CachePercentDirty |  The file share's contribution to the overall percentage of the gateway's cache that has not been persisted to AWS\. The sample is taken at the end of the reporting period\. Use the `CachePercentDirty` metric of the gateway to view the overall percentage of the gateway's cache that has not been persisted to AWS\. Units: Percent  | 
| CachePercentUsed |  The file share's contribution to the overall percent use of the gateway's cache storage\. The sample is taken at the end of the reporting period\. Use the `CachePercentUsed` metric of the gateway to view overall percent use of the gateway's cache storage\. Units: Percent  | 
| CloudBytesUploaded |  The total number of bytes that the gateway uploaded to AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\.  Units: Bytes  | 
| CloudBytesDownloaded |  The total number of bytes that the gateway downloaded from AWS during the reporting period\.  Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure input/output operations per second \(IOPS\)\. Units: Bytes  | 
| ReadBytes  |  The total number of bytes read from your on\-premises applications in the reporting period for a file share\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 
| WriteBytes |  The total number of bytes written to your on\-premises applications in the reporting period\. Use this metric with the `Sum` statistic to measure throughput and with the `Samples` statistic to measure IOPS\. Units: Bytes  | 

## Understanding file gateway audit logs<a name="audit-logs"></a>

Amazon FSx File Gateway \(FSx File\) audit logs provide you with details about user access to files and folders within a file system association\. You can use audit logs to monitor user activities and take action if inappropriate activity patterns are identified\. The logs are formatted similar to Windows Server security log events, to support compatibility with existing log processing tools for Windows security events\. 

**Operations**

The following table describes the file gateway audit log file access operations\.


| Operation name | Definition | 
| --- | --- | 
| Read Data | Read the contents of a file\. | 
| Write Data | Change the contents of a file\. | 
| Create | Create a new file or folder\. | 
| Rename | Rename an existing file or folder\. | 
| Delete | Delete a file or folder\. | 
| Write Attributes | Update file or folder metadata \(ACLs, owner, group, permissions\)\. | 

**Attributes**

The following table describes FSx File audit log file access attributes\.


| Attribute | Definition | 
| --- | --- | 
| `securityDescriptor` | Shows the discretionary access control list \(DACL\) set on an object, in SDDL format\. | 
| `sourceAddress` | The IP address of file share client machine\. | 
| `SubjectDomainName` | The Active Directory \(AD\) domain that the client’s account belongs to\. | 
| `SubjectUserName` | The Active Directory user name of the client\. | 
| `source` | The ID of the Storage Gateway `FileSystemAssociation` that is being audited\. | 
| `mtime` | This time that the object's content was modified, set by the client\. | 
| `version` | The version of the audit log format\. | 
| `ObjectType` | Defines whether the object is a file or folder\. | 
| locationDnsName | The FSx File system DNS name\. | 
| `objectName` | The full path to the object\. | 
| `ctime` | The time that the object’s content or metadata was modified, set by the client\. | 
| `shareName` | The name of the share that is being accessed\. | 
| `operation` | The name of the object access operation\. | 
| `newObjectName` | The full path to the new object after it has been renamed\. | 
| `gateway` | The Storage Gateway ID\. | 
| `status` | The status of the operation\. Only success is logged \(failures are logged with the exception of failures arising from permissions denied\)\. | 
| `fileSizeInBytes` | The size of the file in bytes, set by the client at file creation time\. | 

**Attributes logged per operation**

The following table describes the FSx File audit log attributes logged in each file access operation\.


|  | Read data | Write data | Create folder | Create file | Rename file/folder | Delete file/folder | Write attributes \(change ACL\) | Write attributes \(chown\) | Write attributes \(chmod\) | Write attributes \(chgrp\) | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| `securityDescriptor` |  |  |  |  |  |  | X |  |  |  | 
| `sourceAddress` | X | X | X | X | X | X | X | X | X | X | 
| `SubjectDomainName` | X | X | X | X | X | X | X | X | X | X | 
| `SubjectUserName` | X | X | X | X | X | X | X | X | X | X | 
| `source` | X | X | X | X | X | X | X | X | X | X | 
| `mtime` |  |  | X | X |  |  |  |  |  |  | 
| `version` | X | X | X | X | X | X | X | X | X | X | 
| `objectType` | X | X | X | X | X | X | X | X | X | X | 
| `locationDnsName` | X | X | X | X | X | X | X | X | X | X | 
| `objectName` | X | X | X | X | X | X | X | X | X | X | 
| `ctime` |  |  | X | X |  |  |  |  |  |  | 
| `shareName` | X | X | X | X | X | X | X | X | X | X | 
| `operation` | X | X | X | X | X | X | X | X | X | X | 
| `newObjectName` |  |  |  |  | X |  |  |  |  |  | 
| `gateway` | X | X | X | X | X | X | X | X | X | X | 
| `status` | X | X | X | X | X | X | X | X | X | X | 
| `fileSizeInBytes` |  |  |  | X |  |  |  |  |  |  | 