--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Activating a gateway in a virtual private cloud<a name="gateway-private-link"></a>

You can create a private connection between your on\-premises gateway appliance and cloud\-based storage infrastructure\. You can use this connection to activate your gateway and enable it to transfer data to AWS storage services without communicating over the public internet\. Using the Amazon VPC service, you can launch AWS resources, including private network interface endpoints, in a custom virtual private cloud \(VPC\)\. A VPC gives you control over network settings such as IP address range, subnets, route tables, and network gateways\. For more information about VPCs, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

To activate your gateway in a VPC, use the Amazon VPC Console to create a VPC endpoint for Storage Gateway and get the VPC endpoint ID, then specify this VPC endpoint ID when you create and activate the gateway\. For more information, see [Connect your Amazon FSx File Gateway to AWS](https://docs.aws.amazon.com/filegateway/latest/filefsxw/create-gateway-file.html#connect-to-amazon-fsx-file)\.

To enable your FSx File Gateway to transfer data through the VPC, you must establish a VPN or AWS DirectConnect link between the Amazon FSx for Windows File Server VPC and the network where your gateway is deployed\.

**Note**  
You must activate your gateway in the same region where you create the VPC endpoint for Storage Gateway\.

**Topics**
+ [Creating a VPC endpoint for Storage Gateway](#create-vpc-endpoint)

## Creating a VPC endpoint for Storage Gateway<a name="create-vpc-endpoint"></a>

Follow these instructions to create a VPC endpoint\. If you already have a VPC endpoint for Storage Gateway, you can use it\.<a name="create-vpc-steps"></a>

**To create a VPC endpoint for Storage Gateway**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation pane, choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, choose **AWS Services** for **Service category**\.

1. For **Service Name**, choose `com.amazonaws.region.storagegateway`\. For example `com.amazonaws.us-east-2.storagegateway`\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is not selected\.

1. For **Security group**, choose the security group that you want to use for your VPC\. You can accept the default security group\. Verify that all of the following TCP ports are allowed in your security group:
   + TCP 443
   + TCP 1026
   + TCP 1027
   + TCP 1028
   + TCP 1031
   + TCP 2222

1. Choose **Create endpoint**\. The initial state of the endpoint is **pending**\. When the endpoint is created, note the ID of the VPC endpoint that you just created\.

1. When the endpoint is created, choose **Endpoints**, then choose the new VPC endpoint\.

1. In the **DNS Names** section, use the first DNS name that doesn't specify an Availability Zone\. Your DNS name look similar to this: `vpce-1234567e1c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com `

Now that you have a VPC endpoint, you can create and activate your gateway\. For more information, see [Create and activate an Amazon FSx File Gateway](https://docs.aws.amazon.com/filegateway/latest/filefsxw/create-gateway-file.html)\.