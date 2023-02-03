--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Basic performance guidance for FSx File Gateway<a name="performance-fgw"></a>

In this section, you can find guidance for provisioning hardware for your FSx File Gateway VM\. The instance configurations that are listed in the table are examples, and are provided for reference\.

For best performance, the cache disk size must be tuned to the size of the active working set\. Using multiple local disks for the cache increases write performance by parallelizing access to data and leads to higher IOPS\.

**Note**  
We don't recommend using ephemeral storage\. For information about using ephemeral storage, see [Using ephemeral storage with EC2 gateways](ManagingLocalStorage-common.md#ephemeral-disk-cache)\.

In the following tables, *cache hit* read operations are reads from the file data that is served from cache\. *Cache miss* read operations are reads from the file data that is served from Amazon FSx for Windows File Server\.

The following table shows an example FSx File Gateway configuration\.

## FSx File Gateway performance on Windows clients<a name="performance-fgw-fsx-windows"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/performance-fgw.html)

**Note**  
Your performance might vary based on your host platform configuration and network bandwidth\. Write throughput performance decreases with file size, with the highest achievable throughput for small files being 16 files per second\.