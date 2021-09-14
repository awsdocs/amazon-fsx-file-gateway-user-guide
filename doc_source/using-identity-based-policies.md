# Using identity\-based policies \(IAM policies\) for Storage Gateway<a name="using-identity-based-policies"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your Storage Gateway resources\. For more information, see [Overview of managing access permissions to your Storage Gateway](managing-access-overview.md)\. 

The sections in this topic cover the following:
+ [Permissions required to use the Storage Gateway console](#additional-console-required-permissions)
+ [AWS managed policies for Storage Gateway](#access-policy-examples-aws-managed)
+ [Customer managed policy examples](#customer-managed-policies)

The following shows an example of a permissions policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsSpecifiedActionsOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "storagegateway:ActivateGateway",
                "storagegateway:ListGateways"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowsSpecifiedEC2ActionsOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot"
            ],
            "Resource": "*"
        }
    ]
}
```

The policy has two statements \(note the `Action` and `Resource` elements in both the statements\):
+ The first statement grants permissions for two Storage Gateway actions \(`storagegateway:ActivateGateway` and `storagegateway:ListGateways`\) on a gateway resource\. 

  The wildcard character \(\*\) means that this statement can match any resource\. In this case, the statement allows the `storagegateway:ActivateGateway` and `storagegateway:ListGateways` actions on any gateway\. The wildcard character is used here because you don't know the resource ID until after you create the gateway\. For information about how to use a wildcard character \(\*\) in a policy, see [Example 2: Allow read\-only access to a gateway](#sg-example2)\.
**Note**  
ARNs uniquely identify AWS resources\. For more information, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *AWS General Reference*\.

  To limit permissions for a particular action to a specific gateway only, create a separate statement for that action in the policy and specify the gateway ID in that statement\. 

   
+ The second statement grants permissions for the `ec2:DescribeSnapshots` and `ec2:DeleteSnapshot` actions\. These Amazon Elastic Compute Cloud \(Amazon EC2\) actions require permissions because snapshots generated from Storage Gateway are stored in Amazon Elastic Block Store \(Amazon EBS\) and managed as Amazon EC2 resources, and thus they require corresponding EC2 actions\. For more information, see [Actions](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_Operations.html) in the *Amazon EC2 API Reference*\. Because these Amazon EC2 actions don't support resource\-level permissions, the policy specifies the wildcard character \(\*\) as the `Resource` value instead of specifying a gateway ARN\. 

For a table showing all of the Storage Gateway API actions and the resources that they apply to, see [Storage Gateway API permissions: Actions, resources, and conditions reference](sg-api-permissions-ref.md)\. 

## Permissions required to use the Storage Gateway console<a name="additional-console-required-permissions"></a>

To use the Storage Gateway console, you need to grant read\-only permissions\. If you plan to describe snapshots, you also need to grant permissions for additional actions as shown in the following permissions policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsSpecifiedEC2ActionOnAllGateways",
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Resource": "*"
        }
    ]
}
```

This additional permission is required because the Amazon EBS snapshots generated from Storage Gateway are managed as Amazon EC2 resources\. 

To set up the minimum permissions required to navigate the Storage Gateway console, see [Example 2: Allow read\-only access to a gateway](#sg-example2)\.

## AWS managed policies for Storage Gateway<a name="access-policy-examples-aws-managed"></a>

Amazon Web Services addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information about AWS managed policies, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to Storage Gateway:

 
+ **AWSStorageGatewayReadOnlyAccess** – Grants read\-only access to Storage Gateway resources\. 
+ **AWSStorageGatewayFullAccess** – Grants full access to Storage Gateway resources\. 

 

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for Storage Gateway API actions\. You can attach these custom policies to the IAM users or groups that require those permissions\.

## Customer managed policy examples<a name="customer-managed-policies"></a>

In this section, you can find example user policies that grant permissions for various Storage Gateway actions\. These policies work when you are using AWS SDKs and the AWS CLI\. When you are using the console, you need to grant additional permissions specific to the console, which is discussed in [Permissions required to use the Storage Gateway console](#additional-console-required-permissions)\.

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

**Topics**
+ [Example 1: Allow any Storage Gateway actions on all gateways](#sg-example1)
+ [Example 2: Allow read\-only access to a gateway](#sg-example2)
+ [Example 3: Allow access to a specific gateway](#sg-example3)
+ [Example 4: Allow a user to access a specific volume](#sg-example4)
+ [Example 5: Allow all actions on gateways with a specific prefix](#sg-example5)

### Example 1: Allow any Storage Gateway actions on all gateways<a name="sg-example1"></a>

The following policy allows a user to perform all the Storage Gateway actions\. The policy also allows the user to perform Amazon EC2 actions \([DescribeSnapshots](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshots.html) and [DeleteSnapshot](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DeleteSnapshot.html)\) on the Amazon EBS snapshots generated from Storage Gateway\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsAllAWSStorageGatewayActions",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {You can use Windows ACLs only with file shares that are enabled for Active Directory. 
            "Sid": "AllowsSpecifiedEC2Actions",
            "Action": [
                "ec2:DescribeSnapshots",
                "ec2:DeleteSnapshot"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 2: Allow read\-only access to a gateway<a name="sg-example2"></a>

The following policy allows all `List*` and `Describe*` actions on all resources\. Note that these actions are read\-only actions\. Thus, the policy doesn't allow the user to change the state of any resources—that is, the policy doesn't allow the user to perform actions such as `DeleteGateway`, `ActivateGateway`, and `ShutdownGateway`\.

The policy also allows the `DescribeSnapshots` Amazon EC2 action\. For more information, see [DescribeSnapshots](https://docs.aws.amazon.com/storagegateway/latest/APIReference/API_DescribeSnapshots.html) in the *Amazon EC2 API Reference*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowReadOnlyAccessToAllGateways",
            "Action": [
                "storagegateway:List*",
                "storagegateway:Describe*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsUserToDescribeSnapshotsOnAllGateways",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

In the preceding policy, instead of a using a wildcard character \(\*\), you can scope resources covered by the policy to a specific gateway, as shown in the following example\. The policy then allows the actions only on the specific gateway\.

 

```
"Resource": [
              "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/", 
              "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
```

Within a gateway, you can further restrict the scope of the resources to only the gateway volumes, as shown in the following example: 

 

```
"Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/*" 
```

### Example 3: Allow access to a specific gateway<a name="sg-example3"></a>

The following policy allows all actions on a specific gateway\. The user is restricted from accessing other gateways you might have deployed\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowReadOnlyAccessToAllGateways",
            "Action": [
                "storagegateway:List*",
                "storagegateway:Describe*"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsUserToDescribeSnapshotsOnAllGateways",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Sid": "AllowsAllActionsOnSpecificGateway",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/",
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
        }
    ]
}
```

The preceding policy works if the user to which the policy is attached uses either the API or an AWS SDK to access the gateway\. However, if the user is going to use the Storage Gateway console, you must also grant permissions to allow the `ListGateways` action, as shown in the following example\.

 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsAllActionsOnSpecificGateway",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/",
                "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/*"
            ]
        },
        {
            "Sid": "AllowsUserToUseAWSConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 4: Allow a user to access a specific volume<a name="sg-example4"></a>

The following policy allows a user to perform all actions to a specific volume on a gateway\. Because a user doesn't get any permissions by default, the policy restricts the user to accessing only a specific volume\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GrantsPermissionsToSpecificVolume",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/volume-id"
        },
        {
            "Sid": "GrantsPermissionsToUseStorageGatewayConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

The preceding policy works if the user to whom the policy is attached uses either the API or an AWS SDK to access the volume\. However, if this user is going to use the Storage Gateway console, you must also grant permissions to allow the `ListGateways` action, as shown in the following example\.

 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "GrantsPermissionsToSpecificVolume",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/gateway-id/volume/volume-id"
        },
        {
            "Sid": "GrantsPermissionsToUseStorageGatewayConsole",
            "Action": [
                "storagegateway:ListGateways"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

### Example 5: Allow all actions on gateways with a specific prefix<a name="sg-example5"></a>

The following policy allows a user to perform all Storage Gateway actions on gateways with names that start with `DeptX`\. The policy also allows the `DescribeSnapshots` Amazon EC2 action which is required if you plan to describe snapshots\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowsActionsGatewayWithPrefixDeptX",
            "Action": [
                "storagegateway:*"
            ],
            "Effect": "Allow",
            "Resource": "arn:aws:storagegateway:us-west-2:123456789012:gateway/DeptX"
        },
        {
            "Sid": "GrantsPermissionsToSpecifiedAction",
            "Action": [
                "ec2:DescribeSnapshots"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

The preceding policy works if the user to whom the policy is attached uses either the API or an AWS SDK to access the gateway\. However, if this user plans to use the Storage Gateway console, you must grant additional permissions as described in [Example 3: Allow access to a specific gateway](#sg-example3)\.