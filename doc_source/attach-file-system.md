--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Attaching an Amazon FSx file system<a name="attach-file-system"></a>

You must have an FSx for Windows File Server file system before you can attach it to an FSx File Gateway\. If you don't have a file system, you must create one\. For instructions, see [Step 1: Create Your File System](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started-step1.html) in the *Amazon FSx for Windows File Server User Guide*\.

The next step is to activate an FSx File Gateway and configure your gateway to join an Active Directory domain\. For instructions, see [Configure Active Directory settings](join-domain-fsx.md)\.

**Note**  
When your gateway has joined a domain, you don't have to configure it to join the domain again\.

Each gateway can support up to five attached file systems\. For instructions on how to attach a file system, see [Attach an Amazon FSx for Windows File Server file system](attach-fsxw-filesystem.md)\.