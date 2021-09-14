# Step 1: Create an FSx for Windows File Server file system<a name="create-file-system"></a>

To create an Amazon FSx File Gateway in Storage Gateway, the first step is to create an Amazon FSx for Windows File Server file system\. If you have already created an Amazon FSx file system, go to the next step, [Step 2: \(Optional\) Create an Amazon VPC endpoint](create-vpc-endpoint-fsx.md)\.

**Note**  
The following limitations apply when writing to an Amazon FSx file system from FSx File gateway:  
Your Amazon FSx file system and your FSx File gateway must be owned by the same AWS account and located in the same AWS Region\.
Each gateway can support 5 attached file systems\. When attaching a file system, the Storage Gateway console notifies you if the selected gateway is at capacity, in which case you must choose a different gateway or detach a file system before you can attach another one\.
FSx File gateway supports soft storage quotas \(warning when users surpass their data limits\), but does not support hard quotas \(enforcing data limits by denying write access\)\. Soft quotas are supported for all users except the Amazon FSx admin user\. For more information on setting up storage quotas, see [Storage quotas](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/managing-user-quotas.html) in the Amazon FSx User Guide\.

**To create an FSx for Windows File Server file system**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/fsx/home/](https://console.aws.amazon.com/fsx/home/), and choose the Region that you want to create your gateway in\.

1. Follow the instructions in [Getting Started with Amazon FSx](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started.html) in the *FSx for Windows File Server User Guide*