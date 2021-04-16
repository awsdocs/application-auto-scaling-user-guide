# Example policies for working with the AWS CLI or an SDK<a name="security_iam_id-based-policy-examples"></a>

By default, a brand new IAM user has no permissions to do anything\. An IAM administrator must create IAM policies that grant users and roles permission to perform Application Auto Scaling actions, such as configuring scaling policies\. The administrator must then attach those policies to the IAM users or roles that require the permissions\.

To learn how to create an IAM policy using the following example JSON policy documents, see [Creating policies on the JSON tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Permissions required for Application Auto Scaling actions](#application-auto-scaling-actions-permissions)
+ [Permissions required for actions on target services and CloudWatch](#application-auto-scaling-additional-permissions)
+ [Permissions required to create a service\-linked role](#application-auto-scaling-slr-permissions)

## Permissions required for Application Auto Scaling actions<a name="application-auto-scaling-actions-permissions"></a>

The following policies grant permissions for common use cases\. Refer to this section when setting up [Access control](auth-and-access-control.md#access-control) and writing permissions policies that you can attach to an IAM user or role\. Each policy grants access to all or some of the Application Auto Scaling actions\. You also need to make sure that the IAM user or role has a permissions policy for the target service and CloudWatch \(see the next section for details\)\. 

The following permissions policy grants access to all Application Auto Scaling actions\.

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

The following permissions policy grants access to all Application Auto Scaling actions that are required to configure scaling policies and not scheduled actions\.

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

The following permissions policy grants access to all Application Auto Scaling actions that are required to configure scheduled actions and not scaling policies\.

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

## Permissions required for actions on target services and CloudWatch<a name="application-auto-scaling-additional-permissions"></a>

To successfully configure and use Application Auto Scaling with the target service, IAM users must be granted the required permissions for Amazon CloudWatch and for each target service for which they will configure scaling\. Use the following policies to give users the minimum permissions required to work with target services and CloudWatch\.

**ECS services**

The following permissions policy grants access to all ECS and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "ecs:DescribeServices",
              "ecs:UpdateService",
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**EC2 Spot Fleet requests**

The following permissions policy grants access to all Spot Fleet and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "ec2:DescribeSpotFleetRequests",
              "ec2:ModifySpotFleetRequest",
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Amazon EMR clusters**

The following permissions policy grants access to all Amazon EMR and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "elasticmapreduce:ModifyInstanceGroups",
              "elasticmapreduce:ListInstanceGroups",
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**AppStream 2\.0 fleets**

The following permissions policy grants access to all AppStream 2\.0 and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "appstream:DescribeFleets",
              "appstream:UpdateFleet",
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**DynamoDB tables and global secondary indexes**

The following permissions policy grants access to all DynamoDB and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "dynamodb:DescribeTable",
              "dynamodb:UpdateTable",
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Aurora DB clusters**

The following permissions policy grants access to all Aurora and CloudWatch actions that are required\.

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
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**SageMaker endpoints**

The following permissions policy grants access to all SageMaker and CloudWatch actions that are required\.

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
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Custom resources**

The following permissions policy grants a user the required permission for the API Gateway API executing action\. This policy also grants access to all CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "execute-api:Invoke",          
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Amazon Comprehend document classification endpoints**

The following permissions policy grants access to all Amazon Comprehend and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "comprehend:UpdateEndpoint",
              "comprehend:DescribeEndpoint",           
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Lambda functions**

The following permissions policy grants access to all Lambda and CloudWatch actions that are required\.

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
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

**Amazon Keyspaces tables**

The following permissions policy grants access to all Amazon Keyspaces and CloudWatch actions that are required\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "cassandra:Select",
              "cassandra:Alter",             
              "cloudwatch:DeleteAlarms",
              "cloudwatch:DescribeAlarms",
              "cloudwatch:PutMetricAlarm"
            ],
            "Resource": "*"
        }
    ]
}
```

## Permissions required to create a service\-linked role<a name="application-auto-scaling-slr-permissions"></a>

Application Auto Scaling requires permissions to create a service\-linked role the first time any user in your AWS account calls `RegisterScalableTarget` for a given service\. Application Auto Scaling creates a service\-linked role for the target service in your account, if the role does not exist already\. The service\-linked role grants permissions to Application Auto Scaling so that it can call the target service on your behalf\. 

For automatic role creation to succeed, users must have permission for the `iam:CreateServiceLinkedRole` action\.

```
"Action": "iam:CreateServiceLinkedRole"
```

The following example is a permissions policy that allows an IAM user or role to create an Application Auto Scaling service\-linked role for Spot Fleet\. You can specify the service\-linked role in the policy's `Resource` field as an ARN, and the service principal for your service\-linked role as a condition, as shown\. For a complete list of ARNs for Application Auto Scaling, see [Service\-linked role ARN reference](application-auto-scaling-service-linked-roles.md#specify-service-linked-role)\.

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
The `iam:AWSServiceName` IAM condition key specifies the service principal to which the role is attached, which is indicated in this example policy as `ec2.application-autoscaling.amazonaws.com`\. Do not try to guess the service principal\. To view the service principal for a service, see the [service\-linked role documentation](application-auto-scaling-service-linked-roles.md)\. 