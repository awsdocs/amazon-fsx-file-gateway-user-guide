# File gateway setup requirements<a name="Requirements"></a>

Unless otherwise noted, the following requirements are common to all file gateway types in Storage Gateway\. Your setup must meet the requirements in this section\. Review the requirements that apply to your gateway setup before you deploy your gateway\.

**Topics**
+ [Required prerequisites](#user-requirements)
+ [Hardware and storage requirements](#requirements-hardware-storage)
+ [Network and firewall requirements](#networks)
+ [Supported hypervisors and host requirements](#requirements-host)
+ [Supported SMB clients for a file gateway](#requirements-smb-versions)
+ [Supported file system operations for a file gateway](#requirements-file-operations)

## Required prerequisites<a name="user-requirements"></a>

Before you use an Amazon FSx File Gateway \(FSx File\), you must meet the following requirements:
+ Create and configure an FSx for Windows File Server file system\. For instructions, see [Step 1: Create Your File System](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/getting-started-step1.html) in the *FSx for Windows File Server User Guide*\.
+ Configure Microsoft Active Directory \(AD\)\.
+ Ensure that there is sufficient network bandwidth between the gateway and AWS\. A minimum of 100 Mbps is required to successfully download, activate, and update the gateway\.
+ Configure your private networking, VPN, or AWS Direct Connect between your Amazon Virtual Private Cloud \(Amazon VPC\) and the on\-premises environment where you are deploying your FSx File\.
+ Make sure your gateway can resolve the name of your Active Directory Domain Controller\. You can use DHCP in your Active Directory domain to handle resolution, or specify a DNS server manually from the Network Configuration settings menu in the gateway local console\.

## Hardware and storage requirements<a name="requirements-hardware-storage"></a>

The following sections provide information about the minimum required hardware and settings for your gateway, and the minimum amount of disk space to allocate for the required storage\.

### Hardware requirements for on\-premises VMs<a name="requirements-hardware"></a>

When deploying your gateway on\-premises, ensure that the underlying hardware on which you deploy the gateway virtual machine \(VM\) can dedicate the following minimum resources:
+ Four virtual processors assigned to the VM
+ 16 GiB of reserved RAM for file gateways
+ 80 GiB of disk space for installation of VM image and system data

### Requirements for Amazon EC2 instance types<a name="requirements-hardware-ec2"></a>

When deploying your gateway on Amazon Elastic Compute Cloud \(Amazon EC2\), the instance size must be at least **`xlarge`** for your gateway to function\. However, for the compute\-optimized instance family the size must be at least **`2xlarge`**\. Use one of the following instance types recommended for your gateway type\.

**Recommended for file gateway types**
+ General\-purpose instance family – m4 or m5 instance type\.
+ Compute\-optimized instance family – c4 or c5 instance types\. Choose the **2xlarge** instance size or higher to meet the required RAM requirements\.
+ Memory\-optimized instance family – r3 instance types\.
+ Storage\-optimized instance family – i3 instance types\.
**Note**  
When you launch your gateway in Amazon EC2 and the instance type you choose supports ephemeral storage, the disks are listed automatically\. For more information about Amazon EC2 instance storage, see [Instance storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html) in the *Amazon EC2 User Guide\.* 

### Storage requirements<a name="requirements-storage"></a>

In addition to 80 GiB of disk space for the VM, you also need additional disks for your gateway\.


| Gateway type | Cache \(minimum\) | Cache \(maximum\) | 
| --- | --- | --- | 
| File gateway | 150 GiB | 64 TiB | 

**Note**  
You can configure one or more local drives for your cache, up to the maximum capacity\.  
When adding cache to an existing gateway, it's important to create new disks in your host \(hypervisor or Amazon EC2 instance\)\. Don't change the size of existing disks if the disks have been previously allocated as a cache\.

## Network and firewall requirements<a name="networks"></a>

Your gateway requires access to the internet, local networks, Domain Name Service \(DNS\) servers, firewalls, routers, and so on\.

Network bandwidth requirements vary based on the quantity of data that is uploaded and downloaded by the gateway\. A minimum of 100Mbps is required to successfully download, activate, and update the gateway\. Your data transfer patterns will determine the bandwidth necessary to support your workload\.

Following, you can find information about required ports and how to allow access through firewalls and routers\.

**Note**  
In some cases, you might deploy FSx File on Amazon EC2 or use other types of deployment \(including on\-premises\) with network security policies that restrict AWS IP address ranges\. In these cases, your gateway might experience service connectivity issues when the AWS IP range values changes\. The AWS IP address range values that you need to use are in the Amazon service subset for the AWS Region that you activate your gateway in\. For the current IP range values, see [AWS IP address ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html) in the *AWS General Reference*\.

**Topics**
+ [Port requirements](#requirements-network)
+ [Networking and firewall requirements for the Storage Gateway Hardware Appliance](#appliance-network-requirements)
+ [Allowing Storage Gateway access through firewalls and routers](#allow-firewall-gateway-access)
+ [Configuring security groups for your Amazon EC2 gateway instance](#EC2GatewayCustomSecurityGroup-common)

### Port requirements<a name="requirements-network"></a>



**Common ports for all gateway types**

The following ports are common to all gateway types and are required by all gateway types\.


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How used  | 
| --- | --- | --- | --- | --- | --- | 
|  TCP  |  443 \(HTTPS\)  |  Outbound  |  Storage Gateway  |  AWS  |  For communication from Storage Gateway to the AWS service endpoint\. For information about service endpoints, see [Allowing Storage Gateway access through firewalls and routers](#allow-firewall-gateway-access)\.  | 
|  TCP  |  80 \(HTTP\)  |  Inbound  |  The host from which you connect to the AWS Management Console\.  |  Storage Gateway  |  By local systems to obtain the storage gateway activation key\. Port 80 is only used during activation of the Storage Gateway appliance\.  Storage Gateway does not require port 80 to be publicly accessible\. The required level of access to port 80 depends on your network configuration\. If you activate your gateway from the Storage Gateway console, the host from which you connect to the console must have access to your gateway’s port 80\.  | 
|  UDP/UDP  |  53 \(DNS\)  |  Outbound  |  Storage Gateway  |  DNS server  |  For communication between Storage Gateway and the DNS server\.  | 
|  TCP  |  22 \(Support channel\)  |  Outbound  |  Storage Gateway  |  AWS Support  |  Allows AWS Support to access your gateway to help you with troubleshooting gateway issues\. You don't need this port open for the normal operation of your gateway, but it is required for troubleshooting\.  | 
|  UDP  |  123 \(NTP\)  |  Outbound  |  NTP client  |  NTP server  |  Used by local systems to synchronize VM time to the host time\.   | 

**Ports for file gateways**

For FSx File, you must use Microsoft Active Directory to allow domain users to access a Server Message Block \(SMB\) file share\. You can join your file gateway to any valid Microsoft Windows domain \(resolvable by DNS\)\.

You can also use the AWS Directory Service to create an [AWS Managed Microsoft AD](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_microsoft_ad.html) in the Amazon Web Services Cloud\. For most AWS Managed Microsoft AD deployments, you need to configure the Dynamic Host Configuration Protocol \(DHCP\) service for your VPC\. For information about creating a DHCP options set, see [Create a DHCP options set](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/dhcp_options_set.html) in the *AWS Directory Service Administration Guide*\.

FSx File requires the following ports\. 


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How used  | 
| --- | --- | --- | --- | --- | --- | 
|  UDP NetBIOS  |  137  |  Inbound and outbound  |    |  Microsoft Active Directory  |  For connecting to Microsoft Active Directory\.  | 
|  UDP NetBIOS  |  138  |  Inbound and outbound  |    |  | For Datagram service | 
|  TCP LDAP   |  389  |  Inbound and outbound  |    |    |  For Directory System Agent \(DSA\) client connection  | 
|  TCP v2/v3 data  |  445  |  Outbound  |    |    |  Storage data transfer between file gateway and FSx for Windows File Server  | 
|  TCP \(HTTPS\)  |  443  |  Outbound  |    |  Storage Gateway service endpoints  |  Management control – Used for communication from an Storage Gateway VM to an AWS service endpoint  | 
|  TCP HTTPS  |  443  |  Outbound  |  |  Amazon CloudFront  |  For gateway activation  | 
|  TCP  |  443  |  Outbound  |    |  VPC endpoint usage  |  Management control – Used for communication from an Storage Gateway VM to an AWS service endpoint\.  | 
|  TCP  |  1026  |  Outbound  |    |    |  Used for control traffic  | 
|  TCP  |  1027  | Outbound |    |    |  Used only during activation and can then be closed  | 
|  TCP  | 1028 | Outbound |    |    | Used for control traffic | 
|  TCP  |  1031  | Outbound |    |    |  Used only for software updates for file gateways  | 
|  TCP  |  2222  |  Outbound  |    |    |  Used to open a support channel to the gateway when using VPC endpoints  | 
|  TCP \(HTTPS\)  |  8080  |  Inbound  |    |    |  Required briefly for activation of a hardware appliance  | 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/images/xxxxx.png)

### Networking and firewall requirements for the Storage Gateway Hardware Appliance<a name="appliance-network-requirements"></a>

Each Storage Gateway Hardware Appliance requires the following network services:
+ **Internet access** – an always\-on network connection to the internet through any network interface on the server\.
+ **DNS services** – DNS services for communication between the hardware appliance and DNS server\.
+ **Time synchronization** – an automatically configured Amazon NTP time service must be reachable\.
+ **IP address** – A DHCP or static IPv4 address assigned\. You cannot assign an IPv6 address\.

There are five physical network ports at the rear of the Dell PowerEdge R640 server\. From left to right \(facing the back of the server\) these ports are as follows:

1. iDRAC

1. `em1`

1. `em2`

1. `em3`

1. `em4`

You can use the iDRAC port for remote server management\.



![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/images/ApplianceFirewallRules.png)

A hardware appliance requires the following ports to operate\.


|  Protocol  |  Port  |  Direction  |  Source  |  Destination  |  How used  | 
| --- | --- | --- | --- | --- | --- | 
| SSH |  22  |  Outbound  | Hardware appliance |  `54.201.223.107`  | Support channel | 
| DNS | 53 | Outbound | Hardware appliance | DNS servers | Name resolution | 
| UDP/NTP | 123 | Outbound | Hardware appliance | \*\.amazon\.pool\.ntp\.org | Time synchronization | 
| HTTPS |  443  |  Outbound  | Hardware appliance |  `*.amazonaws.com`  |  Data transfer  | 
| HTTP | 8080 | Inbound | AWS | Hardware appliance | Activation \(only briefly\) | 

To perform as designed, a hardware appliance requires network and firewall settings as follows:
+ Configure all connected network interfaces in the hardware console\.
+ Make sure that each network interface is on a unique subnet\.
+ Provide all connected network interfaces with outbound access to the endpoints listed in the diagram preceding\.
+ Configure at least one network interface to support the hardware appliance\. For more information, see [Configuring network parameters](appliance-configure-network.md)\.

**Note**  
For an illustration showing the back of the server with its ports, see [Rack\-mounting your hardware appliance and connecting it to power](appliance-rack-mount.md)\.

All IP addresses on the same network interface \(NIC\), whether for a gateway or a host, must be on the same subnet\. The following illustration shows the addressing scheme\.



![\[Image NOT FOUND\]](http://docs.aws.amazon.com/filegateway/latest/filefsxw/images/ApplianceAddressing.png)

For more information about activating and configuring a hardware appliance, see [Using the Storage Gateway Hardware Appliance](hardware-appliance.md)\.

### Allowing Storage Gateway access through firewalls and routers<a name="allow-firewall-gateway-access"></a>

Your gateway requires access to the following service endpoints to communicate with AWS\. If you use a firewall or router to filter or limit network traffic, you must configure your firewall and router to allow these service endpoints for outbound communication to AWS\.

**Important**  
Depending on your gateway's AWS Region, replace *region* in the service endpoint with the correct Region string\.

The following service endpoints are required by all gateways for control path \(`anon-cp`, `client-cp`, `proxy-app`\) and data path \(`dp-1`\) operations\.

```
anon-cp.storagegateway.region.amazonaws.com:443
client-cp.storagegateway.region.amazonaws.com:443
proxy-app.storagegateway.region.amazonaws.com:443
dp-1.storagegateway.region.amazonaws.com:443
```

The following gateway service endpoint is required to make API calls\.

```
storagegateway.region.amazonaws.com:443
```

The following example is a gateway service endpoint in the US West \(Oregon\) Region \(`us-west-2`\)\.

```
storagegateway.us-west-2.amazonaws.com:443
```

The Amazon CloudFront endpoint following is required for Storage Gateway to get the list of available AWS Regions\.

```
https://d4kdq0yaxexbo.cloudfront.net/
```

A Storage Gateway VM is configured to use the following NTP servers\.

```
0.amazon.pool.ntp.org
1.amazon.pool.ntp.org
2.amazon.pool.ntp.org
3.amazon.pool.ntp.org
```
+ Storage Gateway—For supported AWS Regions and a list of AWS service endpoints that you can use with Storage Gateway, see [Storage Gateway endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sg.html) in the *AWS General Reference*\.
+ Storage Gateway Hardware Appliance—For supported AWS Regions that you can use with the hardware appliance, see [Storage Gateway hardware appliance Regions](https://docs.aws.amazon.com/general/latest/gr/sg.html#sg-hardware-appliance) in the *AWS General Reference*\.



### Configuring security groups for your Amazon EC2 gateway instance<a name="EC2GatewayCustomSecurityGroup-common"></a>

In Storage Gateway, a security group controls traffic to your Amazon EC2 gateway instance\. When you configure a security group, we recommend the following:
+ The security group should not allow incoming connections from the outside internet\. It should allow only instances within the gateway security group to communicate with the gateway\. 

  If you need to allow instances to connect to the gateway from outside its security group, we recommend that you allow connections only on port 80 \(for activation\)\.
+ If you want to activate your gateway from an Amazon EC2 host outside the gateway security group, allow incoming connections on port 80 from the IP address of that host\. If you cannot determine the activating host's IP address, you can open port 80, activate your gateway, and then close access on port 80 after completing activation\.
+ Allow port 22 access only if you are using AWS Support for troubleshooting purposes\. For more information, see [You want AWS Support to help troubleshoot your EC2 gateway](troubleshooting-EC2-gateway-issues.md#EC2-EnableAWSSupportAccess)\.

## Supported hypervisors and host requirements<a name="requirements-host"></a>

You can run Storage Gateway on\-premises as either a virtual machine \(VM\) appliance or a physical hardware appliance, or in AWS as an Amazon EC2 instance\.

Storage Gateway supports the following hypervisor versions and hosts:
+ VMware ESXi Hypervisor \(version 6\.0, 6\.5 or 6\.7\) – A free version of VMware is available on the [VMware website](http://www.vmware.com/products/vsphere-hypervisor/overview.html)\. For this setup, you also need a VMware vSphere client to connect to the host\.
+  Microsoft Hyper\-V Hypervisor \(version 2012 R2 or 2016\) – A free, standalone version of Hyper\-V is available at the [Microsoft Download Center](http://www.microsoft.com/en-us/search/Results.aspx?q=hyper-V&form=DLC)\. For this setup, you need a Microsoft Hyper\-V Manager on a Microsoft Windows client computer to connect to the host\.
+ Linux Kernel\-based Virtual Machine \(KVM\) – A free, open\-source virtualization technology\. KVM is included in all versions of Linux version 2\.6\.20 and newer\. Storage Gateway is tested and supported for the CentOS/RHEL 7\.7, Ubuntu 16\.04 LTS, and Ubuntu 18\.04 LTS distributions\. Any other modern Linux distribution may work, but function or performance is not guaranteed\. We recommend this option if you already have a KVM environment up and running and you are already familiar with how KVM works\.
+ Amazon EC2 instance – Storage Gateway provides an Amazon Machine Image \(AMI\) that contains the gateway VM image\. For information about how to deploy a gateway on Amazon EC2, see [Deploying a file gateway on an Amazon EC2 host](ec2-gateway-file.md)\.
+ Storage Gateway Hardware Appliance – Storage Gateway provides a physical hardware appliance as an on\-premises deployment option for locations with limited virtual machine infrastructure\.

**Note**  
Storage Gateway doesn’t support recovering a gateway from a VM that was created from a snapshot or clone of another gateway VM or from your Amazon EC2 AMI\. If your gateway VM malfunctions, activate a new gateway and recover your data to that gateway\. For more information, see [Recovering from an unexpected virtual machine shutdown](recover-data-from-gateway.md#recover-from-gateway-shutdown)\.  
Storage Gateway doesn’t support dynamic memory and virtual memory ballooning\.

## Supported SMB clients for a file gateway<a name="requirements-smb-versions"></a>

File gateways support the following Service Message Block \(SMB\) clients:
+ Microsoft Windows Server 2008 and later
+ Windows desktop versions: 10, 8, and 7\.
+  Windows Terminal Server running on Windows Server 2008 and later
**Note**  
 Server Message Block encryption requires clients that support SMB v2\.1\.

## Supported file system operations for a file gateway<a name="requirements-file-operations"></a>

Your SMB client can write, read, delete, and truncate ﬁles\. When clients send writes to Storage Gateway, it writes to local cache synchronously\. Then it writes to Amazon FSx asynchronously through optimized transfers\. Reads are first served through the local cache\. If data is not available, it's fetched through Amazon FSx as a read\-through cache\.

Writes and reads are optimized in that only the parts that are changed or requested are transferred through your gateway\. Deletes remove files from Amazon FSx\.