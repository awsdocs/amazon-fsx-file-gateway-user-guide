--------

Amazon S3 File Gateway documentation has been moved to [What is Amazon S3 File Gateway?](https://docs.aws.amazon.com/filegateway/latest/files3/WhatIsStorageGateway.html)

Volume Gateway documentation has been moved to [What is Volume Gateway?](https://docs.aws.amazon.com/storagegateway/latest/vgw/WhatIsStorageGateway.html)

Tape Gateway documentation has been moved to [What is Tape Gateway?](https://docs.aws.amazon.com/storagegateway/latest/tgw/WhatIsStorageGateway.html)

--------

# Step 2: \(Optional\) Create an Amazon VPC endpoint<a name="create-vpc-endpoint-fsx"></a>

This step is not required when you are creating an Amazon FSx File Gateway in AWS Storage Gateway\. However, we recommend that you create a virtual private cloud \(VPC\) endpoint for Storage Gateway and activate the gateway in the VPC\. Doing so creates a private connection between your VPC and Storage Gateway\. 

If you already have a VPC endpoint for Storage Gateway, you can use it for your FSx File Gateway\. A single VPC endpoint that can support multiple gateways allows gateways deployed in your VPC to connect to the Storage Gateway service VPC\. If you have already created a VPC endpoint for Storage Gateway, go to the next step, [Step 3: Create and activate an Amazon FSx File Gateway](create-gateway-file.md)\.

**To create an Amazon VPC endpoint**

1. Open the AWS Management Console at [https://console\.aws\.amazon\.com/vpc/home/](https://console.aws.amazon.com/vpc/home/), and choose the AWS Region that you want to create your gateway in\.

1. In the left navigation pane, choose **Endpoints**, and then choose **Create endpoint**\.

1. On the **Create endpoint** page, choose **AWS services** for **Service category**\.

1. For **Service name**, search for `storagegateway`\. The Region will default to the Region that you are signed in to—for example, `com.amazonaws.region.storagegateway`\. So if you are signed in to US East \(Ohio\), you would see `com.amazonaws.us-east-2.storagegateway`\.

1. For **VPC**, choose your VPC and note its Availability Zones and subnets\.

1. Verify that **Enable Private DNS Name** is not selected\.

1. For **Security group**, create a new security group to use with your VPC\. Make sure that all of the following TCP ports are allowed in your security group:
   + TCP 1026
   + TCP 1027
   + TCP 1028
   + TCP 1031
   + TCP 2222
**Note**  
The gateway uses these ports to communicate back to the Storage Gateway managed service\. When you are using a VPC endpoint, the following ports must be open for inbound access from the IP address of your gateway\.

1. Choose **Create endpoint**\. The initial state of the endpoint is **Pending**\. When the endpoint is created, take note of the ID of the VPC endpoint that you just created\.
**Note**  
We recommend that you provide a name for this VPC endpoint, for example, **StorageGatewayEndpoint**\.

1. When the endpoint is created, choose **Endpoints**, and then choose the new **VPC endpoint**\.

1. In the **DNS Names** section, use the first Domain Name System \(DNS\) name that doesn't specify an Availability Zone\. Your DNS name should look similar to the following: 

   `vpce-1234567e1c24a1fe9-62qntt8k.storagegateway.us-east-1.vpce.amazonaws.com`
**Note**  
This DNS name will resolve to the Storage Gateway endpoint private IP addresses that are allocated in your VPC\.

1. Review the list of ports that must be opened on your firewall\. 

Now that you have created a VPC endpoint, you can create your FSx File Gateway\.

**Next step**  
[Step 3: Create and activate an Amazon FSx File Gateway](create-gateway-file.md)