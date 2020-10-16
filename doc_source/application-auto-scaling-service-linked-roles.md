# Service\-linked roles for Application Auto Scaling<a name="application-auto-scaling-service-linked-roles"></a>

Application Auto Scaling uses service\-linked roles for the permissions that it requires to call other AWS services on your behalf\. A service\-linked role is a unique type of AWS Identity and Access Management \(IAM\) role that is linked directly to an AWS service\. 

Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. For more information, see [Using service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\. 

The following sections describe how to create and manage Application Auto Scaling service\-linked roles\. Start by configuring permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\.

**Topics**
+ [Permissions granted by the service\-linked roles](#service-linked-role-permissions)
+ [Create service\-linked roles \(automatic\)](#create-service-linked-role-automatic)
+ [Create service\-linked roles \(manual\)](#create-service-linked-role-manual)
+ [Edit the service\-linked roles](#edit-service-linked-role)
+ [Delete the service\-linked roles](#delete-service-linked-role)
+ [Supported regions for Application Auto Scaling service\-linked roles](#slr-regions)
+ [Service\-linked role ARN reference](#specify-service-linked-role)

## Permissions granted by the service\-linked roles<a name="service-linked-role-permissions"></a>

Application Auto Scaling uses the following service\-linked roles to manage scaling on your behalf\. There is one service\-linked role per type of scalable resource\. In each case, the service\-linked role is a predefined role that includes all of the necessary permissions\. Each service\-linked role trusts the specified service principal to assume it\.

**AppStream 2\.0 fleets**  
The `AWSServiceRoleForApplicationAutoScaling_AppStreamFleet` service\-linked role trusts the `appstream.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `appstream:DescribeFleets`
+ Action: `appstream:UpdateFleet`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Aurora replicas**  
The `AWSServiceRoleForApplicationAutoScaling_RDSCluster` service\-linked role trusts the `rds.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `rds:AddTagsToResource`
+ Action: `rds:CreateDBInstance`
+ Action: `rds:DeleteDBInstance`
+ Action: `rds:DescribeDBClusters`
+ Action: `rds:DescribeDBInstance`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Amazon Comprehend document classification and entity recognizer endpoints**  
The `AWSServiceRoleForApplicationAutoScaling_ComprehendEndpoint` service\-linked role trusts the `comprehend.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `comprehend:UpdateEndpoint`
+ Action: `comprehend:DescribeEndpoint`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**DynamoDB tables and global secondary indexes**  
The `AWSServiceRoleForApplicationAutoScaling_DynamoDBTable` service\-linked role trusts the `dynamodb.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `dynamodb:DescribeTable`
+ Action: `dynamodb:UpdateTable`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Amazon Elastic Container Service \(ECS\) services**  
The `AWSServiceRoleForApplicationAutoScaling_ECSService` service\-linked role trusts the `ecs.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `ecs:DescribeServices`
+ Action: `ecs:UpdateService`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Amazon Keyspaces \(for Apache Cassandra\) tables**  
The `AWSServiceRoleForApplicationAutoScaling_CassandraTable` service\-linked role trusts the `cassandra.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on the specified resources:
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system/table/*`
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system_schema/table/*`
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system_schema_mcs/table/*`
+ Action: `cassandra:Alter` on the resource `arn:*:cassandra:*:*:"*"`
+ Action: `cloudwatch:DeleteAlarms` on all resources
+ Action: `cloudwatch:DescribeAlarms` on all resources
+ Action: `cloudwatch:PutMetricAlarm` on all resources

**Lambda function provisioned concurrency**  
The `AWSServiceRoleForApplicationAutoScaling_LambdaConcurrency` service\-linked role trusts the `lambda.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `lambda:PutProvisionedConcurrencyConfig`
+ Action: `lambda:GetProvisionedConcurrencyConfig`
+ Action: `lambda:DeleteProvisionedConcurrencyConfig`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Amazon Managed Streaming for Apache Kafka \(Amazon MSK\) broker storage**  
The `AWSServiceRoleForApplicationAutoScaling_KafkaCluster` service\-linked role trusts the `kafka.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `kafka:DescribeCluster`
+ Action: `kafka:DescribeClusterOperation`
+ Action: `kafka:UpdateBrokerStorage`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**SageMaker endpoint variants**  
The `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint` service\-linked role trusts the `sagemaker.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `sagemaker:DescribeEndpoint`
+ Action: `sagemaker:DescribeEndpointConfig`
+ Action: `sagemaker:UpdateEndpointWeightsAndCapacities`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Spot Fleet requests**  
The `AWSServiceRoleForApplicationAutoScaling_EC2SpotFleetRequest` service\-linked role trusts the `ec2.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `ec2:DescribeSpotFleetRequests`
+ Action: `ec2:ModifySpotFleetRequest`
+ Action: `cloudwatch:DeleteAlarms`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`

**Custom resources**  
The `AWSServiceRoleForApplicationAutoScaling_CustomResource` service\-linked role trusts the `custom-resource.application-autoscaling.amazonaws.com` service principal to assume the role\. 

The role permissions policy allows Application Auto Scaling to complete the following actions on all resources:
+ Action: `execute-api:Invoke`
+ Action: `cloudwatch:DeleteAlarms `
+ Action: `cloudwatch:DescribeAlarms `
+ Action: `cloudwatch:PutMetricAlarm `

## Create service\-linked roles \(automatic\)<a name="create-service-linked-role-automatic"></a>

You don't need to manually create a service\-linked role\. Application Auto Scaling creates the appropriate service\-linked role for you when you call `RegisterScalableTarget`\. For example, if you set up automatic scaling for an Amazon ECS service, Application Auto Scaling creates the `AWSServiceRoleForApplicationAutoScaling_ECSService` role\.

**Important**  
The IAM user calling the `RegisterScalableTarget` action must have the appropriate IAM permissions to create the service\-linked role\. Otherwise, the automatic creation fails\. For more information, see [Permissions required to create a service\-linked role](security_iam_id-based-policy-examples.md#application-auto-scaling-slr-permissions) in this guide\.

You automatically create a service\-linked role using an AWS CloudFormation template by specifying the `RoleARN` property\. To specify the service\-linked role in the `RoleARN` property, use its full Amazon Resource Name \(ARN\)\. The ARN has the following general syntax:

```
arn:aws:[service]:[region]:[account]:resourceType/resourcePath
```

For more information, see [Service\-linked role ARN reference](#specify-service-linked-role)\.

## Create service\-linked roles \(manual\)<a name="create-service-linked-role-manual"></a>

To create the service\-linked role, you can use the IAM console, AWS CLI, or IAM API\. For more information, see [Creating a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. 

**To create a service\-linked role \(AWS CLI\)**  
Use the following [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) CLI command to create the Application Auto Scaling service\-linked role\. In the request, specify a value for "prefix"\. For example, to create the AWS Lambda service\-linked role, use `lambda.application-autoscaling.amazonaws.com`\.

```
aws iam create-service-linked-role --aws-service-name prefix.amazonaws.com
```

## Edit the service\-linked roles<a name="edit-service-linked-role"></a>

With the service\-linked roles created by Application Auto Scaling, you can edit only their descriptions\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Delete the service\-linked roles<a name="delete-service-linked-role"></a>

If you no longer use Application Auto Scaling with a supported service, we recommend that you delete the corresponding service\-linked role\. 

You can delete a service\-linked role only after first deleting the related AWS resources\. This protects you from inadvertently revoking Application Auto Scaling permissions to your resources\. For more information, see the [documentation](https://docs.aws.amazon.com/) for the scalable resource\. For example, to delete an ECS service, see [Deleting a service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/delete-service.html) in the *Amazon Elastic Container Service Developer Guide*\.

You can use IAM to delete a service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

After you delete a service\-linked role, Application Auto Scaling creates the role again when you call `RegisterScalableTarget`\.

## Supported regions for Application Auto Scaling service\-linked roles<a name="slr-regions"></a>

Application Auto Scaling supports using service\-linked roles in all of the AWS Regions where the service is available\.

## Service\-linked role ARN reference<a name="specify-service-linked-role"></a>

AWS CloudFormation requires you to specify an IAM role in your stack templates when registering scalable targets\. The following service\-linked role ARNs are defined by this service and can be used in the `RoleARN` property of an [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) in your stack template\.


****  

| Resource types | ARN | 
| --- | --- | 
|  AppStream 2\.0 fleets  | arn:aws:iam::012345678910:role/aws\-service\-role/appstream\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_AppStreamFleet | 
| Aurora DB clusters | arn:aws:iam::012345678910:role/aws\-service\-role/rds\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_RDSCluster | 
| Comprehend document classification and entity recognizer endpoints | arn:aws:iam::012345678910:role/aws\-service\-role/comprehend\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_ComprehendEndpoint | 
| DynamoDB tables and global secondary indexes | arn:aws:iam::012345678910:role/aws\-service\-role/dynamodb\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_DynamoDBTable | 
| ECS services | arn:aws:iam::012345678910:role/aws\-service\-role/ecs\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_ECSService | 
| Amazon Keyspaces tables | arn:aws:iam::012345678910:role/aws\-service\-role/cassandra\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_CassandraTable | 
| Lambda function provisioned concurrency | arn:aws:iam::012345678910:role/aws\-service\-role/lambda\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_LambdaConcurrency | 
| Amazon MSK broker storage | arn:aws:iam::012345678910:role/aws\-service\-role/kafka\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_KafkaCluster | 
| SageMaker endpoint variants | arn:aws:iam::012345678910:role/aws\-service\-role/sagemaker\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_SageMakerEndpoint | 
| Spot Fleet requests | arn:aws:iam::012345678910:role/aws\-service\-role/ec2\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_EC2SpotFleetRequest | 
| Custom resources | arn:aws:iam::012345678910:role/aws\-service\-role/custom\-resource\.application\-autoscaling\.amazonaws\.com/AWSServiceRoleForApplicationAutoScaling\_CustomResource | 