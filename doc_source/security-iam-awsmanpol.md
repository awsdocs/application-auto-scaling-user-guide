# AWS managed policies for Application Auto Scaling<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ViewOnlyAccess` AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

**Topics**
+ [AWS managed policy granting access to AppStream 2\.0 and CloudWatch](#appstream-policy)
+ [AWS managed policy granting access to Aurora and CloudWatch](#aurora-policy)
+ [AWS managed policy granting access to Amazon Comprehend and CloudWatch](#comprehend-policy)
+ [AWS managed policy granting access to DynamoDB and CloudWatch](#ddb-policy)
+ [AWS managed policy granting access to Amazon ECS and CloudWatch](#ecs-policy)
+ [AWS managed policy granting access to ElastiCache and CloudWatch](#elasticache-policy)
+ [AWS managed policy granting access to Amazon Keyspaces and CloudWatch](#keyspaces-policy)
+ [AWS managed policy granting access to Lambda and CloudWatch](#lambda-policy)
+ [AWS managed policy granting access to Amazon MSK and CloudWatch](#msk-policy)
+ [AWS managed policy granting access to Neptune and CloudWatch](#neptune-policy)
+ [AWS managed policy granting access to SageMaker and CloudWatch](#sagemaker-policy)
+ [AWS managed policy granting access to EC2 Spot Fleet and CloudWatch](#spot-policy)
+ [AWS managed policy granting access to your custom resources and CloudWatch](#custom-resources-policy)
+ [Application Auto Scaling updates to AWS managed policies](#policy-updates)

## AWS managed policy granting access to AppStream 2\.0 and CloudWatch<a name="appstream-policy"></a>

**Policy name: [AWSApplicationAutoscalingAppStreamFleetPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingAppStreamFleetPolicy)**  
You can't attach `AWSApplicationAutoscalingAppStreamFleetPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon AppStream and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_AppStreamFleet` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `appstream:DescribeFleets`
+ Action: `appstream:UpdateFleet`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Aurora and CloudWatch<a name="aurora-policy"></a>

**Policy name: [AWSApplicationAutoscalingRDSClusterPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingRDSClusterPolicy)**  
You can't attach `AWSApplicationAutoscalingRDSClusterPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Aurora and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_RDSCluster` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `rds:AddTagsToResource`
+ Action: `rds:CreateDBInstance`
+ Action: `rds:DeleteDBInstance`
+ Action: `rds:DescribeDBClusters`
+ Action: `rds:DescribeDBInstance`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Amazon Comprehend and CloudWatch<a name="comprehend-policy"></a>

**Policy name: [AWSApplicationAutoscalingComprehendEndpointPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingComprehendEndpointPolicy)**  
You can't attach `AWSApplicationAutoscalingComprehendEndpointPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon Comprehend and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_ComprehendEndpoint` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `comprehend:UpdateEndpoint`
+ Action: `comprehend:DescribeEndpoint`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to DynamoDB and CloudWatch<a name="ddb-policy"></a>

**Policy name: [AWSApplicationAutoscalingDynamoDBTablePolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingDynamoDBTablePolicy)**  
You can't attach `AWSApplicationAutoscalingDynamoDBTablePolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call DynamoDB and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_DynamoDBTable` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `dynamodb:DescribeTable`
+ Action: `dynamodb:UpdateTable`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Amazon ECS and CloudWatch<a name="ecs-policy"></a>

**Policy name: [AWSApplicationAutoscalingECSServicePolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingECSServicePolicy)**  
You can't attach `AWSApplicationAutoscalingECSServicePolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon ECS and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_ECSService` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `ecs:DescribeServices`
+ Action: `ecs:UpdateService`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to ElastiCache and CloudWatch<a name="elasticache-policy"></a>

**Policy name: [AWSApplicationAutoscalingElastiCacheRGPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingElastiCacheRGPolicy)**  
You can't attach `AWSApplicationAutoscalingElastiCacheRGPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call ElastiCache and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_ElastiCacheRG` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on the specified resources:
+ Action: `elasticache:DescribeReplicationGroups` on all resources
+ Action: `elasticache:ModifyReplicationGroupShardConfiguration` on all resources
+ Action: `elasticache:IncreaseReplicaCount` on all resources
+ Action: `elasticache:DecreaseReplicaCount` on all resources
+ Action: `elasticache:DescribeCacheClusters` on all resources
+ Action: `elasticache:DescribeCacheParameters` on all resources
+ Action: `cloudwatch:DescribeAlarms` on all resources
+ Action: `cloudwatch:PutMetricAlarm` on the resource `arn:*:cloudwatch:*:*:alarm:TargetTracking*`
+ Action: `cloudwatch:DeleteAlarms` on the resource `arn:*:cloudwatch:*:*:alarm:TargetTracking*`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Amazon Keyspaces and CloudWatch<a name="keyspaces-policy"></a>

**Policy name: [AWSApplicationAutoscalingCassandraTablePolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingCassandraTablePolicy)**  
You can't attach `AWSApplicationAutoscalingCassandraTablePolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon Keyspaces and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_CassandraTable` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on the specified resources:
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system/table/*`
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system_schema/table/*`
+ Action: `cassandra:Select` on the resource `arn:*:cassandra:*:*:/keyspace/system_schema_mcs/table/*`
+ Action: `cassandra:Alter` on the resource `arn:*:cassandra:*:*:"*"`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Lambda and CloudWatch<a name="lambda-policy"></a>

**Policy name: [AWSApplicationAutoscalingLambdaConcurrencyPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingLambdaConcurrencyPolicy)**  
You can't attach `AWSApplicationAutoscalingLambdaConcurrencyPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Lambda and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_LambdaConcurrency` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `lambda:PutProvisionedConcurrencyConfig`
+ Action: `lambda:GetProvisionedConcurrencyConfig`
+ Action: `lambda:DeleteProvisionedConcurrencyConfig`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Amazon MSK and CloudWatch<a name="msk-policy"></a>

**Policy name: [AWSApplicationAutoscalingKafkaClusterPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingKafkaClusterPolicy)**  
You can't attach `AWSApplicationAutoscalingKafkaClusterPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon MSK and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_KafkaCluster` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `kafka:DescribeCluster`
+ Action: `kafka:DescribeClusterOperation`
+ Action: `kafka:UpdateBrokerStorage`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to Neptune and CloudWatch<a name="neptune-policy"></a>

**Policy name: [AWSApplicationAutoscalingNeptuneClusterPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingNeptuneClusterPolicy)**  
You can't attach `AWSApplicationAutoscalingNeptuneClusterPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Neptune and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_NeptuneCluster` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on the specified resources:
+ Action: `rds:AddTagsToResource` on resources with the prefix *autoscaled\-reader* in the Amazon Neptune database engine \(`"Condition":{"StringEquals":{"rds:DatabaseEngine":"neptune"}`\)
+ Action: `rds:ListTagsForResource` on all resources
+ Action: `rds:CreateDBInstance` on resources with the prefix *autoscaled\-reader* in all DB clusters \(`"Resource":"arn:*:rds:*:*:db:autoscaled-reader*", "arn:aws:rds:*:*:cluster:*"`\) in the Amazon Neptune database engine \(`"Condition":{"StringEquals":{"rds:DatabaseEngine":"neptune"}`\)
+ Action: `rds:DescribeDBInstances` on all resources
+ Action: `rds:DescribeDBClusters` on all resources
+ Action: `rds:DescribeDBClusterParameters` on all resources
+ Action: `rds:DeleteDBInstance` on the resource `arn:*:rds:*:*:db:autoscaled-reader*`
+ Action: `cloudwatch:DescribeAlarms` on all resources
+ Action: `cloudwatch:PutMetricAlarm` on the resource `arn:*:cloudwatch:*:*:alarm:TargetTracking*`
+ Action: `cloudwatch:DeleteAlarms` on the resource `arn:*:cloudwatch:*:*:alarm:TargetTracking*`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to SageMaker and CloudWatch<a name="sagemaker-policy"></a>

**Policy name: [AWSApplicationAutoscalingSageMakerEndpointPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingSageMakerEndpointPolicy)**  
You can't attach `AWSApplicationAutoscalingSageMakerEndpointPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call SageMaker and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `sagemaker:DescribeEndpoint`
+ Action: `sagemaker:DescribeEndpointConfig`
+ Action: `sagemaker:UpdateEndpointWeightsAndCapacities`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to EC2 Spot Fleet and CloudWatch<a name="spot-policy"></a>

**Policy name: [AWSApplicationAutoscalingEC2SpotFleetRequestPolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoscalingEC2SpotFleetRequestPolicy)**  
You can't attach `AWSApplicationAutoscalingEC2SpotFleetRequestPolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Amazon EC2 and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_EC2SpotFleetRequest` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `ec2:DescribeSpotFleetRequests`
+ Action: `ec2:ModifySpotFleetRequest`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## AWS managed policy granting access to your custom resources and CloudWatch<a name="custom-resources-policy"></a>

**Policy name: [AWSApplicationAutoScalingCustomResourcePolicy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/aws-service-role/AWSApplicationAutoScalingCustomResourcePolicy)**  
You can't attach `AWSApplicationAutoScalingCustomResourcePolicy` to your IAM identities \(users or roles\)\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call your custom resources that are available through API Gateway and CloudWatch and perform scaling on your behalf\.

**Permission details**  
The `AWSServiceRoleForApplicationAutoScaling_CustomResource` service\-linked role permissions policy allows Application Auto Scaling to complete the following actions on all related resources \("Resource": "\*"\):
+ Action: `execute-api:Invoke`
+ Action: `cloudwatch:DescribeAlarms`
+ Action: `cloudwatch:PutMetricAlarm`
+ Action: `cloudwatch:DeleteAlarms`

## Application Auto Scaling updates to AWS managed policies<a name="policy-updates"></a>

View details about updates to AWS managed policies for Application Auto Scaling since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Application Auto Scaling Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  Application Auto Scaling adds Neptune policy  |  Application Auto Scaling added a new managed policy for Neptune\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call Neptune and CloudWatch and perform scaling on your behalf\.  | October 6, 2021 | 
|  Application Auto Scaling adds ElastiCache for Redis policy  |  Application Auto Scaling added a new managed policy for ElastiCache\. This policy is attached to a service\-linked role that allows Application Auto Scaling to call ElastiCache and CloudWatch and perform scaling on your behalf\.  | August 19, 2021 | 
|  Application Auto Scaling started tracking changes  |  Application Auto Scaling started tracking changes for its AWS managed policies\.  | August 19, 2021 | 