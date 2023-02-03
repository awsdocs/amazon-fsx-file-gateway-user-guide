--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Document history for the Amazon FSx File Gateway User Guide<a name="doc-history"></a>
+ **API version**: 2013\-06\-30
+ **Latest documentation update**: July 07, 2021

The following table describes the documentation releases for Amazon FSx File Gateway\. For notification about updates to this documentation, you can subscribe to an RSS feed\.

| Change | Description | Date | 
| --- |--- |--- |
| [Added GatewayClockOutOfSync troubleshooting tips](#doc-history) | The *Troubleshooting: File Gateway issues section* now includes troubleshooting guidelines to help diagnose problems you may encounter if your gateway system clock is not synchronized with the AWS Storage Gateway server time\. For more information, see [Error: GatewayClockOutOfSync](https://docs.aws.amazon.com/filegateway/latest/filefsxw/troubleshooting-file-gateway-issues.html#troubleshoot-logging-errors-gatewayclockoutofsync)\. | October 19, 2022 | 
| [Added Active Directory Join Domain troubleshooting tips](#doc-history) | The *Troubleshooting: File Gateway issues section* now includes troubleshooting guidelines to help diagnose problems you may encounter when trying to join your gateway to an Active Directory domain\. For more information, see [Troubleshooting: Active Directory domain issues](https://docs.aws.amazon.com/filegateway/latest/filefsxw/troubleshooting-file-gateway-issues.html#troubleshooting-ad-domain)\. | October 19, 2022 | 
| [Multiple file system support](#doc-history) | Amazon FSx File Gateway now supports up to five attached Amazon FSx file systems\. For more information, see [Attach an Amazon FSx for Windows File Server file system](https://docs.aws.amazon.com/filegateway/latest/filefsxw/attach-fsxw-filesystem.html)\. | July 7, 2021 | 
| [Amazon FSx soft storage quota support](#doc-history) | Amazon FSx File Gateway now supports soft storage quotas \(which warn you when users surpass their data limits\) when writing to attached Amazon FSx file systems where storage quotas are configured\. Hard quotas \(which enforce data limits by denying write access\) are not supported\. Soft quotas work for all users except the Amazon FSx admin user\. For more information about setting up storage quotas, see [Storage quotas](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/managing-user-quotas.html) in the *Amazon FSx for Windows File Server User Guide*\. | July 7, 2021 | 
| [New guide](#doc-history) | In addition to the original File Gateway \(now known as Amazon S3 File Gateway\), Storage Gateway provides Amazon FSx File Gateway \(FSx File Gateway\)\. FSx File Gateway provides low latency and efficient access to in\-cloud FSx for Windows File Server file shares from your on\-premises facility\. For more information, see [What is Amazon FSx File Gateway?](https://docs.aws.amazon.com/filegateway/latest/filefsxw/what-is-file-fsxw.html) | April 27, 2021 | 