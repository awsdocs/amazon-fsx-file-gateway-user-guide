--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Quotas<a name="fgw-quotas"></a>

## Quotas for file systems<a name="resource-file-limits-fsx"></a>

The following table lists quotas for file systems\.


| Resource | Limit per file system | 
| --- | --- | 
| Maximum number of tags | 50 | 
| Maximum retention period for automated backups | 90 days | 
| Maximum number of backup copy requests in progress to a single destination Region per account\.  | 5 | 
| Minimum storage capacity, SSD file systems | 32 GiB | 
| Minimum storage capacity, HDD file systems | 2,000 GiB | 
| Maximum storage capacity, SSD and HDD | 64 TiB | 
| Minimum throughput capacity | 8 MBps | 
| Maximum throughput capacity | 2,048 MBps | 
| Maximum number of file shares | 100,000 | 

## Recommended local disk sizes for your gateway<a name="disk-sizes-fsx"></a>

The following table recommends sizes for local disk storage for your deployed gateway\. 


| Gateway Type | Cache \(Minimum\) | Cache \(Maximum\) | Other Required Local Disks | 
| --- | --- | --- | --- | 
| FSx File Gateway | 150 GiB | 64 TiB | — | 

**Note**  
You can configure one or more local drives for your cache up to the maximum capacity\.  
When adding cache to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as a cache\.