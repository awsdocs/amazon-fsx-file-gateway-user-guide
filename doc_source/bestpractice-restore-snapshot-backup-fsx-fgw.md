--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Best practices: restoring from backups or snapshots directly on your Amazon FSx file system<a name="bestpractice-restore-snapshot-backup-fsx-fgw"></a>

In some cases, you might need to restore data on your Amazon FSx file system directly, using a backup or snapshot from an earlier point in time\. In these instances, there is a risk of creating a dual\-writer scenario between the backup application and the FSx File Gateway, which can result in stuck or mis\-matched files\. To avoid problems when restoring your Amazon FSx file system from backups or snapshots, use the following procedure\.

**Note**  
By restoring your Amazon FSx file system from a backup or snapshot using this procedure, you will invalidate any cached data currently stored on your FSx File Gateway\.

**To avoid problems when restoring your Amazon FSx file system from backups or snapshots**

1. Detach the Amazon FSx file system from the FSx File Gateway using the Storage Gateway console\.

1. Restore the backup or snapshot directly on your Amazon FSx file system\.

1. Reattach the Amazon FSx file system to the FSx File Gateway using the Storage Gateway console\.