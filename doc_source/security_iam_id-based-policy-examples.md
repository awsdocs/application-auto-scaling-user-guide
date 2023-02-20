# Application Auto Scaling identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, a brand new user in your AWS account has no permissions to do anything\. An IAM administrator must create and assign IAM policies that give an IAM identity \(such as a user or role\) permission to perform Application Auto Scaling API actions\.

To learn how to create an IAM policy using the following example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Permissions required for Application Auto Scaling API actions](#application-auto-scaling-actions-permissions)
+ [Permissions required for API actions on target services and CloudWatch](#application-auto-scaling-additional-permissions)
+ [Permissions for working in the AWS Management Console](#security_iam_id-based-policy-examples-console)

## Permissions required for Application Auto Scaling API actions<a name="application-auto-scaling-actions-permissions"></a>

The following policies grant permissions for common use cases when calling Application Auto Scaling API\. Refer to this section when writing identity\-based policies\. Each policy grants permissions to all or some of the Application Auto Scaling API actions\. You also need to make sure that end users have permissions for the target service and CloudWatch \(see the next section for details\)\. 

The following identity\-based policy grants permissions to all Application Auto Scaling API actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "application-autoscaling:*"
            ],
            "Resource": "*"
        }
    ]
}
```

The following identity\-based policy grants permissions to all Application Auto Scaling API actions that are required to configure scaling policies and not scheduled actions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "application-autoscaling:RegisterScalableTarget",
              "application-autoscaling:DescribeScalableTargets",
              "application-autoscaling:DeregisterScalableTarget",
              "application-autoscaling:PutScalingPolicy",
              "application-autoscaling:DescribeScalingPolicies",
              "application-autoscaling:DescribeScalingActivities",
              "application-autoscaling:DeleteScalingPolicy"
            ],
            "Resource": "*"
        }
    ]
}
```

The following identity\-based policy grants permissions to all Application Auto Scaling API actions that are required to configure scheduled actions and not scaling policies\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "application-autoscaling:RegisterScalableTarget",
              "application-autoscaling:DescribeScalableTargets",
              "application-autoscaling:DeregisterScalableTarget",
              "application-autoscaling:PutScheduledAction",
              "application-autoscaling:DescribeScheduledActions",
              "application-autoscaling:DescribeScalingActivities",
              "application-autoscaling:DeleteScheduledAction"
            ],
            "Resource": "*"
        }
    ]
}
```

## Permissions required for API actions on target services and CloudWatch<a name="application-auto-scaling-additional-permissions"></a>

To successfully configure and use Application Auto Scaling with the target service, end users must be granted permissions for Amazon CloudWatch and for each target service for which they will configure scaling\. Use the following policies to grant the minimum permissions required to work with target services and CloudWatch\.

**Topics**
+ [AppStream 2\.0 fleets](#permissions-for-appstream-fleets)
+ [Aurora replicas](#permissions-for-aurora-clusters)
+ [Amazon Comprehend document classification and entity recognizer endpoints](#permissions-for-comprehend-endpoints)
+ [DynamoDB tables and global secondary indexes](#permissions-for-dynamodb-tables-and-gsis)
+ [ECS services](#permissions-for-ecs-services)
+ [ElastiCache replication groups](#permissions-for-elasticache)
+ [Amazon EMR clusters](#permissions-for-emr-clusters)
+ [Amazon Keyspaces tables](#permissions-for-keyspaces-tables)
+ [Lambda functions](#permissions-for-lambda-functions)
+ [Amazon Managed Streaming for Apache Kafka \(MSK\) broker storage](#permissions-for-msk-broker-storage)
+ [Neptune clusters](#permissions-for-neptune-clusters)
+ [SageMaker endpoints](#permissions-for-sagemaker-endpoints)
+ [Spot Fleets \(Amazon EC2\)](#permissions-for-spot-fleet)
+ [Custom resources](#permissions-for-custom-resources)

### AppStream 2\.0 fleets<a name="permissions-for-appstream-fleets"></a>

The following identity\-based policy grants permissions to all AppStream 2\.0 and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "appstream:DescribeFleets",
              "appstream:UpdateFleet",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Aurora replicas<a name="permissions-for-aurora-clusters"></a>

The following identity\-based policy grants permissions to all Aurora and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "rds:AddTagsToResource",
              "rds:CreateDBInstance",
              "rds:DeleteDBInstance",
              "rds:DescribeDBClusters",
              "rds:DescribeDBInstances",              
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Amazon Comprehend document classification and entity recognizer endpoints<a name="permissions-for-comprehend-endpoints"></a>

The following identity\-based policy grants permissions to all Amazon Comprehend and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "comprehend:UpdateEndpoint",
              "comprehend:DescribeEndpoint",           
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### DynamoDB tables and global secondary indexes<a name="permissions-for-dynamodb-tables-and-gsis"></a>

The following identity\-based policy grants permissions to all DynamoDB and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "dynamodb:DescribeTable",
              "dynamodb:UpdateTable",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### ECS services<a name="permissions-for-ecs-services"></a>

The following identity\-based policy grants permissions to all ECS and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "ecs:DescribeServices",
              "ecs:UpdateService",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### ElastiCache replication groups<a name="permissions-for-elasticache"></a>

The following identity\-based policy grants permissions to all ElastiCache and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "elasticache:ModifyReplicationGroupShardConfiguration",
              "elasticache:IncreaseReplicaCount",
              "elasticache:DecreaseReplicaCount",
              "elasticache:DescribeReplicationGroups",
              "elasticache:DescribeCacheClusters",
              "elasticache:DescribeCacheParameters",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Amazon EMR clusters<a name="permissions-for-emr-clusters"></a>

The following identity\-based policy grants permissions to all Amazon EMR and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "elasticmapreduce:ModifyInstanceGroups",
              "elasticmapreduce:ListInstanceGroups",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Amazon Keyspaces tables<a name="permissions-for-keyspaces-tables"></a>

The following identity\-based policy grants permissions to all Amazon Keyspaces and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "cassandra:Select",
              "cassandra:Alter",             
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Lambda functions<a name="permissions-for-lambda-functions"></a>

The following identity\-based policy grants permissions to all Lambda and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "lambda:PutProvisionedConcurrencyConfig",
              "lambda:GetProvisionedConcurrencyConfig",
              "lambda:DeleteProvisionedConcurrencyConfig",             
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Amazon Managed Streaming for Apache Kafka \(MSK\) broker storage<a name="permissions-for-msk-broker-storage"></a>

The following identity\-based policy grants permissions to all Amazon MSK and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "kafka:DescribeCluster",
              "kafka:DescribeClusterOperation",
              "kafka:UpdateBrokerStorage",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Neptune clusters<a name="permissions-for-neptune-clusters"></a>

The following identity\-based policy grants permissions to all Neptune and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "rds:AddTagsToResource",
              "rds:CreateDBInstance",
              "rds:DescribeDBInstances",
              "rds:DescribeDBClusters",
              "rds:DescribeDBClusterParameters",
              "rds:DeleteDBInstance",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### SageMaker endpoints<a name="permissions-for-sagemaker-endpoints"></a>

The following identity\-based policy grants permissions to all SageMaker and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "sagemaker:DescribeEndpoint",
              "sagemaker:DescribeEndpointConfig",
              "sagemaker:UpdateEndpointWeightsAndCapacities",             
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Spot Fleets \(Amazon EC2\)<a name="permissions-for-spot-fleet"></a>

The following identity\-based policy grants permissions to all Spot Fleet and CloudWatch API actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "ec2:DescribeSpotFleetRequests",
              "ec2:ModifySpotFleetRequest",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

### Custom resources<a name="permissions-for-custom-resources"></a>

The following identity\-based policy grants permission for the API Gateway API executing action\. This policy also grants permissions to all CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "execute-api:Invoke",          
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm",
              "cloudwatch:DeleteAlarms"
            ],
            "Resource": "*"
        }
    ]
}
```

## Permissions for working in the AWS Management Console<a name="security_iam_id-based-policy-examples-console"></a>

There is no standalone Application Auto Scaling console\. Most services that integrate with Application Auto Scaling have features that are dedicated to helping you configure scaling with their console\. 

In most cases, each service provides AWS managed \(predefined\) IAM policies that define access to their console, which includes permissions to the Application Auto Scaling API actions\. For more information, refer to the documentation for the service whose console you want to use\. 

You can also create your own custom IAM policies to give users fine\-grained permissions to view and work with specific Application Auto Scaling API actions in the AWS Management Console\. You can use the example policies in the previous sections; however, they are designed for requests that are made with the AWS CLI or an SDK\. The console uses additional API actions for its features, so these policies may not work as expected\. For example, to configure step scaling, users might require additional permissions to create and manage CloudWatch alarms\. 

**Tip**  
To help you work out which API actions are required to perform tasks in the console, you can use a service such as AWS CloudTrail\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)\. 

The following identity\-based policy grants permissions to configure scaling policies for Spot Fleet\. In addition to the IAM permissions for Spot Fleet, the console user that accesses fleet scaling settings from the Amazon EC2 console must have the appropriate permissions for the services that support dynamic scaling\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:*",
                "ec2:DescribeSpotFleetRequests",
                "ec2:ModifySpotFleetRequest",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarmHistory",
                "cloudwatch:DescribeAlarms",
                "cloudwatch:DescribeAlarmsForMetric",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:ListMetrics",
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DisableAlarmActions",
                "cloudwatch:EnableAlarmActions",
                "sns:CreateTopic",
                "sns:Subscribe",
                "sns:Get*",
                "sns:List*"
            ],
            "Resource": "*"
        },
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

This policy allows console users to view and modify scaling policies in the Amazon EC2 console, and to create and manage CloudWatch alarms in the CloudWatch console\. 

You can adjust the API actions to limit user access\. For example, replacing `application-autoscaling:*` with `application-autoscaling:Describe*` means that the user has read\-only access\. 

You can also adjust the CloudWatch permissions as required to limit user access to CloudWatch features\. For more information, see [Permissions required to use the CloudWatch console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-identity-based-access-control-cw.html#console-permissions-cw) in the *Amazon CloudWatch User Guide*\.