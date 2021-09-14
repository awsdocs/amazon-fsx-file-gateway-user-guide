# How Amazon FSx File Gateway works<a name="file-gateway-fsx-concepts"></a>

To use Amazon FSx File Gateway \(FSx File\), you must have at least one Amazon FSx for Windows File Server file system\. You must also have on\-premises access to FSx for Windows File Server, either through a VPN or through an AWS Direct Connect connection\. For more information about using Amazon FSx file systems, see [What is Amazon FSx for Windows File Server?](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) 

You download and deploy the FSx File VMware virtual appliance or an AWS Storage Gateway Hardware Appliance into your on\-premises environment\. After deploying your appliance, you activate the FSx File from the Storage Gateway console or through the Storage Gateway API\. You can also create an FSx File using an Amazon Elastic Compute Cloud \(Amazon EC2\) image\. 

After the Amazon FSx File Gateway is activated and can access FSx for Windows File Server, use the Storage Gateway console to join it to your Microsoft Active Directory domain\. After the gateway successfully joins a domain, you use the Storage Gateway console to attach the gateway to an existing FSx for Windows File Server\. FSx for Windows File Server makes all the shares on the server available as shares on your Amazon FSx File Gateway\. You can then use a client to browse and connect to the file shares on FSx File that correspond to the selected FSx File\. 

When the file shares are connected, you can read and write your files locally, while benefiting from all the features available on FSx for Windows File Server\. FSx File maps local file shares and their contents to file shares stored remotely in FSx for Windows File Server\. There is a 1:1 correspondence between the remote and locally visible files and their shares\.

The following diagram provides an overview of file storage deployment for Storage Gateway\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/images/file-fsx-architecture.png)

Note the following in the diagram:
+ **AWS Direct Connect or a VPN** is needed to allow the FSx File to access the Amazon FSx file share using SMB and to allow the FSx for Windows File Server to join your on\-premises Active Directory domain\.
+ **Amazon Virtual Private Cloud \(Amazon VPC\)** is needed to connect to the FSx for Windows File Server service VPC and the Storage Gateway service VPC using private endpoints\. The FSx File can also connect to the public endpoints\.

You can use Amazon FSx File Gateway in all AWS Regions where FSx for Windows File Server is available\. 