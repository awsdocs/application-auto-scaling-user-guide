# Service\-linked roles for Application Auto Scaling<a name="application-auto-scaling-service-linked-roles"></a>

Application Auto Scaling uses [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) for the permissions that it requires to call other AWS services on your behalf\. A service\-linked role is a unique type of AWS Identity and Access Management \(IAM\) role that is linked directly to an AWS service\. Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. 

**Topics**
+ [Overview](#application-auto-scaling-role-overview)
+ [Permissions required to create a service\-linked role](#create-service-linked-role-permissions)
+ [Create service\-linked roles \(automatic\)](#create-service-linked-role-automatic)
+ [Create service\-linked roles \(manual\)](#create-service-linked-role-manual)
+ [Edit the service\-linked roles](#edit-service-linked-role)
+ [Delete the service\-linked roles](#delete-service-linked-role)
+ [Supported Regions for Application Auto Scaling service\-linked roles](#slr-regions)
+ [Service\-linked role ARN reference](#specify-service-linked-role)

## Overview<a name="application-auto-scaling-role-overview"></a>

For services that integrate with Application Auto Scaling, Application Auto Scaling creates service\-linked roles for you\. There is one service\-linked role for each service\. Each service\-linked role trusts the specified service principal to assume it\. For more information, see [AWS services that you can use with Application Auto Scaling](integrated-services-list.md)\.

Application Auto Scaling includes all of the necessary permissions for each service\-linked role\. These managed permissions are created and managed by Application Auto Scaling, and they define the allowed actions for each resource type\. For details about the permissions that each role grants, see [AWS managed policies for Application Auto Scaling](security-iam-awsmanpol.md)\.

The following sections describe how to create and manage Application Auto Scaling service\-linked roles\. Start by configuring permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\.

## Permissions required to create a service\-linked role<a name="create-service-linked-role-permissions"></a>

Application Auto Scaling requires permissions to create a service\-linked role the first time any user in your AWS account calls `RegisterScalableTarget` for a given service\. Application Auto Scaling creates a service\-linked role for the target service in your account, if the role does not exist already\. The service\-linked role grants permissions to Application Auto Scaling so that it can call the target service on your behalf\. 

For automatic role creation to succeed, users must have permission for the `iam:CreateServiceLinkedRole` action\.

```
"Action": "iam:CreateServiceLinkedRole"
```

The following is a permissions policy that allows an IAM user or role to create a service\-linked role for Spot Fleet\. You can specify the service\-linked role in the policy's `Resource` field as an ARN, and the service principal for your service\-linked role as a condition, as shown\. For the ARN for each service, see [Service\-linked role ARN reference](#specify-service-linked-role)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/ec2.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_EC2SpotFleetRequest",
            "Condition": {
                "StringLike": {
                    "iam:AWSServiceName":"ec2.application-autoscaling.amazonaws.com"
                }
            }
        }
    ]
}
```

**Note**  
The `iam:AWSServiceName` IAM condition key specifies the service principal to which the role is attached, which is indicated in this example policy as `ec2.application-autoscaling.amazonaws.com`\. Do not try to guess the service principal\. To view the service principal for a service, see [AWS services that you can use with Application Auto Scaling](integrated-services-list.md)\.

## Create service\-linked roles \(automatic\)<a name="create-service-linked-role-automatic"></a>

You don't need to manually create a service\-linked role\. Application Auto Scaling creates the appropriate service\-linked role for you when you call `RegisterScalableTarget`\. For example, if you set up automatic scaling for an Amazon ECS service, Application Auto Scaling creates the `AWSServiceRoleForApplicationAutoScaling_ECSService` role\.

## Create service\-linked roles \(manual\)<a name="create-service-linked-role-manual"></a>

To create the service\-linked role, you can use the IAM console, AWS CLI, or IAM API\. For more information, see [Creating a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. 

**To create a service\-linked role \(AWS CLI\)**  
Use the following [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) CLI command to create the Application Auto Scaling service\-linked role\. In the request, specify the service name "prefix"\. 

To find the service name prefix, refer to the information about the service principal for the service\-linked role for each service in the [AWS services that you can use with Application Auto Scaling](integrated-services-list.md) section\. The service name and the service principal share the same prefix\. For example, to create the AWS Lambda service\-linked role, use `lambda.application-autoscaling.amazonaws.com`\. 

```
aws iam create-service-linked-role --aws-service-name prefix.application-autoscaling.amazonaws.com
```

## Edit the service\-linked roles<a name="edit-service-linked-role"></a>

With the service\-linked roles created by Application Auto Scaling, you can edit only their descriptions\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Delete the service\-linked roles<a name="delete-service-linked-role"></a>

If you no longer use Application Auto Scaling with a supported service, we recommend that you delete the corresponding service\-linked role\. 

You can delete a service\-linked role only after first deleting the related AWS resources\. This protects you from inadvertently revoking Application Auto Scaling permissions to your resources\. For more information, see the [documentation](https://docs.aws.amazon.com/) for the scalable resource\. For example, to delete an Amazon ECS service, see [Deleting a service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/delete-service.html) in the *Amazon Elastic Container Service Developer Guide*\.

You can use IAM to delete a service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

After you delete a service\-linked role, Application Auto Scaling creates the role again when you call `RegisterScalableTarget`\.

## Supported Regions for Application Auto Scaling service\-linked roles<a name="slr-regions"></a>

Application Auto Scaling supports using service\-linked roles in all of the AWS Regions where the service is available\.

## Service\-linked role ARN reference<a name="specify-service-linked-role"></a>


****  

| Service | ARN | 
| --- | --- | 
| AppStream 2\.0 | arn:aws:iam::012345678910:role/aws\-service\-role/appstream\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_AppStreamFleet | 
| Aurora | arn:aws:iam::012345678910:role/aws\-service\-role/rds\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_RDSCluster | 
| Comprehend | arn:aws:iam::012345678910:role/aws\-service\-role/comprehend\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_ComprehendEndpoint | 
| DynamoDB | arn:aws:iam::012345678910:role/aws\-service\-role/dynamodb\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_DynamoDBTable | 
| ECS | arn:aws:iam::012345678910:role/aws\-service\-role/ecs\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_ECSService | 
| ElastiCache | arn:aws:iam::012345678910:role/aws\-service\-role/elasticache\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_ElastiCacheRG | 
| Keyspaces | arn:aws:iam::012345678910:role/aws\-service\-role/cassandra\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_CassandraTable | 
| Lambda | arn:aws:iam::012345678910:role/aws\-service\-role/lambda\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_LambdaConcurrency | 
| MSK | arn:aws:iam::012345678910:role/aws\-service\-role/kafka\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_KafkaCluster | 
| Neptune | arn:aws:iam::012345678910:role/aws\-service\-role/neptune\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_NeptuneCluster | 
| SageMaker | arn:aws:iam::012345678910:role/aws\-service\-role/sagemaker\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_SageMakerEndpoint | 
| Spot Fleets | arn:aws:iam::012345678910:role/aws\-service\-role/ec2\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_EC2SpotFleetRequest | 
| Custom resources | arn:aws:iam::012345678910:role/aws\-service\-role/custom\-resource\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_CustomResource | 

**Note**  
You can specify the ARN of a service\-linked role for the `RoleARN` property of an [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) resource in your AWS CloudFormation stack templates, even if the specified service\-linked role doesn't yet exist\. Application Auto Scaling automatically creates the role for you\.