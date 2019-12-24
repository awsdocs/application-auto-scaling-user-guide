# Application Auto Scaling and Interface VPC Endpoints<a name="application-auto-scaling-vpc-endpoints"></a>

You can establish a private connection between your virtual private cloud \(VPC\) and the Application Auto Scaling API by creating an interface VPC endpoint\. You can use this connection to call the Application Auto Scaling API from your VPC without sending traffic over the internet\. The endpoint provides reliable, scalable connectivity to the Application Auto Scaling API\. It does this without requiring an internet gateway, NAT instance, or VPN connection\. 

Interface VPC endpoints are powered by AWS PrivateLink, a feature that enables private communication between AWS services using private IP addresses\. For more information, see [AWS PrivateLink](https://aws.amazon.com/privatelink)\.

**Note**  
You must explicitly enable each API that you want to access through an interface VPC endpoint\. For example, you might need to also configure an interface VPC endpoint for `autoscaling.region.amazonaws.com` if you're using the Amazon EC2 Auto Scaling API operations\. For more information, see [Amazon EC2 Auto Scaling and Interface VPC Endpoints](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-vpc-endpoints) in the *Amazon EC2 Auto Scaling User Guide*\.

Application Auto Scaling currently supports VPC endpoints in all Regions except China \(Beijing\) and China \(Ningxia\)\.

## Create an Interface VPC Endpoint<a name="create-vpce-app-as"></a>

Create an endpoint for Application Auto Scaling using the following service name:
+ **com\.amazonaws\.*region*\.application\-autoscaling**  — Creates an endpoint for the Application Auto Scaling API operations\. 

For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\. 

**Important**  
Make sure that you're using the latest version of the CLI or SDK before creating an interface VPC endpoint\. For more information, see [Endpoint Migration](#upgrading-cli-sdk-app-as)\.

Enable private DNS for the endpoint to make API requests to the supported service using its default DNS hostname \(for example, `application-autoscaling.us-east-1.amazonaws.com`\)\. When creating an endpoint for AWS services, this setting is enabled by default\. For more information, see [Accessing a Service Through an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\. 

You do not need to change any Application Auto Scaling settings\. Application Auto Scaling calls other AWS services using either public endpoints or private interface VPC endpoints, whichever are in use\. 

## Create a VPC Endpoint Policy<a name="create-vpce-policy-app-as"></a>

You can attach a policy to your VPC endpoint to control access to the Application Auto Scaling API\. The policy specifies:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resource on which the actions can be performed\.

The following example shows a VPC endpoint policy that denies everyone permission to create a scaling policy through the endpoint\. The example policy also grants everyone permission to perform all other actions\.

```
{
   "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "application-autoscaling:PutScalingPolicy",
            "Effect": "Deny",
            "Resource": "*",
            "Principal": "*"
        }
    ]
}
```

For more information, see [Using VPC Endpoint Policies](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html#vpc-endpoint-policies) in the *Amazon VPC User Guide*\.

## Endpoint Migration<a name="upgrading-cli-sdk-app-as"></a>

On November 22, 2019, Application Auto Scaling introduced `application-autoscaling.region.amazonaws.com` as the new default DNS hostname and endpoint for calls to the Application Auto Scaling API\. The new endpoint is compatible with the latest release of the AWS CLI and SDKs\. If you have not done so already, install the latest AWS CLI and SDKs to use the new endpoint\. To update the AWS CLI, see [Installing the AWS CLI Using pip](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html#install-tool-pip) in the *AWS Command Line Interface User Guide*\. For information about the AWS SDKs, see [Tools for Amazon Web Services](https://aws.amazon.com/tools)\.

**Note**  
For backward compatibility, the `autoscaling.region.amazonaws.com` endpoint will continue to be supported for calls to the Application Auto Scaling API\. To set up the `autoscaling.region.amazonaws.com` endpoint as a private interface VPC endpoint, see [Amazon EC2 Auto Scaling and Interface VPC Endpoints](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-vpc-endpoints) in the *Amazon EC2 Auto Scaling User Guide*\.

**Endpoint to Call When Using the CLI or the AWS API**  
For the current release of Application Auto Scaling, your calls to the Application Auto Scaling API automatically go to the `application-autoscaling.region.amazonaws.com` endpoint instead of `autoscaling.region.amazonaws.com`\.

You can call the new endpoint in the CLI by using the following parameter with each command to specify the endpoint: `--endpoint-url https://application-autoscaling.region.amazonaws.com`\. 

You can also call the old endpoint in the CLI by using the following parameter with each command to specify the endpoint: `--endpoint-url https://autoscaling.region.amazonaws.com`\. 

For the various SDKs used to call the APIs, see the documentation for the SDK of interest to learn how to direct the requests to a specific endpoint\. For more information, see [Tools for Amazon Web Services](https://aws.amazon.com/tools)\.