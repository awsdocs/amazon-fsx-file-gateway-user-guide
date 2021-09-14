# Managing local disks for your Storage Gateway<a name="ManagingLocalStorage-common"></a>

The gateway virtual machine \(VM\) uses the local disks that you allocate on\-premises for buffering and storage\. Gateways created on Amazon EC2 instances use Amazon EBS volumes as local disks\. 

**Topics**
+ [Deciding the amount of local disk storage](#decide-local-disks-and-sizes)
+ [Determining the size of cache storage to allocate](#CachedLocalDiskCacheSizing-common)
+ [Adding cache storage](#ConfiguringLocalDiskStorage)

## Deciding the amount of local disk storage<a name="decide-local-disks-and-sizes"></a>

The number and size of disks that you want to allocate for your gateway is up to you\. The gateway requires the following additional storage:

File gateways require at least one disk to use as a cache\. The following table recommends sizes for local disk storage for your deployed gateway\. You can add more local storage later after you set up the gateway, and as your workload demands increase\.


| Local storage | Description | Gateway type | 
| --- | --- | --- | 
| Cache storage | The cache storage acts as the on\-premises durable store for data that is pending upload to Amazon S3 or file system\. |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/ManagingLocalStorage-common.html)  | 

**Note**  
Underlying physical storage resources are represented as a data store in VMware\. When you deploy the gateway VM, you choose a data store on which to store the VM files\. When you provision a local disk \(for example, to use as cache storage\), you have the option to store the virtual disk in the same data store as the VM or a different data store\.  
If you have more than one data store, we strongly recommend that you choose one data store for the cache storage\. A data store that is backed by only one underlying physical disk can lead to poor performance in some situations when it is used to back both the cache storage\. This is also true if the backup is a less\-performant RAID configuration such as RAID1\.

After the initial configuration and deployment of your gateway, you can adjust the local storage by adding disks for cache storage\.

## Determining the size of cache storage to allocate<a name="CachedLocalDiskCacheSizing-common"></a>

You can initially use this approximation to provision disks for the cache storage\. You can then use Amazon CloudWatch operational metrics to monitor the cache storage usage and provision more storage as needed using the console\. For information on using the metrics and setting up alarms, see [Performance](Performance.md)\.

## Adding cache storage<a name="ConfiguringLocalDiskStorage"></a>

As your application needs change, you can increase the gateway's cache storage capacity\. You can add more cache capacity to your gateway without interrupting existing gateway functions\. When you add more storage capacity, you do so with the gateway VM turned on\.

**Important**  
When adding cache to an existing gateway, it is important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as either a cache\. Do not remove cache disks that have been allocated as cache storage\.

 The following procedure shows you how to configure or cache storage for your gateway\.<a name="GatewayWorkingStorageCachedTaskBuffer"></a>

**To add and configure or cache storage**

1. Provision a new disk in your host \(hypervisor or Amazon EC2 instance\)\. For information about how to provision a disk in a hypervisor, see your hypervisor's user manual\. You configure this disk as cache storage\.

1. Open the Storage Gateway console at [https://console\.aws\.amazon\.com/storagegateway/home](https://console.aws.amazon.com/storagegateway/)\.

1. In the navigation pane, choose **Gateways**\.

1. In the **Actions** menu, choose **Edit local disks**\.

1. In the Edit local disks dialog box, identify the disks you provisioned and decide which one you want to use for cached storage\.

   If you don't see your disks, choose the **Refresh** button\.

1. Choose **Save** to save your configuration settings\.

FSx File doesn't support ephemeral storage\.