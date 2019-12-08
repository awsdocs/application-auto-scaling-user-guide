# Authentication and Access Control for Application Auto Scaling<a name="auth-and-access-control"></a>

Access to Application Auto Scaling requires credentials that AWS can use to authenticate your requests\. Those credentials must have [permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) to perform Application Auto Scaling actions, such as configuring scaling policies\.

This topic provides details on how you can use AWS Identity and Access Management \(IAM\) to help secure your resources by controlling who can perform Application Auto Scaling actions\. 

By default, a brand new IAM user has no permissions to do anything\. To grant permissions to call Application Auto Scaling actions, you attach an IAM policy to the IAM users or groups that require the permissions it grants\. 

## Specifying Actions in a Policy<a name="application-auto-scaling-actions"></a>

Application Auto Scaling provides a set of actions that you can specify in an IAM policy\. For more information, see [Actions](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_Operations.html) in the *Application Auto Scaling API Reference*\.

To specify a single policy, you can use the following prefix with the name of the action: `application-autoscaling:`\. For example:

```
"Action": "application-autoscaling:DescribeScalingActivities"
```

Wildcards are supported\. For example, you can use `application-autoscaling:*` to specify all Application Auto Scaling actions\.

```
"Action": "application-autoscaling:*"
```

You can also use `Describe*` to specify all actions whose names start with `Describe`\.

```
"Action": "application-autoscaling:Describe*"
```

In addition to the permissions for calling Application Auto Scaling actions, users also require permissions to create a service\-linked role\.

When users call `RegisterScalableTarget`, Application Auto Scaling creates a service\-linked role in your account, if the role does not exist already\. The service\-linked role grants permissions to Application Auto Scaling, so that it can call other services on your behalf\. 

For automatic role creation to succeed, users must have permissions for the `iam:CreateServiceLinkedRole` action\. 

```
"Action": "iam:CreateServiceLinkedRole"
```

For more information, see [Service\-Linked Roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.

## Specifying the Resource<a name="application-auto-scaling-resources"></a>

Application Auto Scaling has no service\-defined resources that can be used as the `Resource` element of an IAM policy statement\. Therefore, there are no Amazon Resource Names \(ARNs\) for you to use in an IAM policy\. To control access to Application Auto Scaling actions, always use an \* \(asterisk\) as the resource when writing an IAM policy\. 

## Specifying Conditions in a Policy<a name="application-auto-scaling-keys"></a>

When you grant permissions, you can use IAM policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. To express conditions, use predefined condition keys\.

For a list of condition keys supported by each AWS service, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actions-resources-contextkeys.html) in the *IAM User Guide*\. For a list of condition keys that can be used in multiple AWS services, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

Application Auto Scaling does not provide additional condition keys\.

## Example Policies<a name="application-auto-scaling-example-policies"></a>

The following policies grant the permissions for common use cases\. You can attach these policies to your IAM users, based on the access that they need\. Each policy grants access to all or some of the API actions for Application Auto Scaling\. 

To configure scaling policies and scheduled scaling for a scalable resource, users must have permissions to use the actions in the following example policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
              "application-autoscaling:*",
              "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*"
        }
    ]
}
```

To configure scaling policies for a scalable resource, users must have permissions to use the actions in the following example policy:

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
              "application-autoscaling:DeleteScalingPolicy",
              "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*"
        }
    ]
}
```

To configure scheduled scaling for a scalable resource, users must have permissions to use the actions in the following example policy:

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
              "application-autoscaling:DeleteScheduledAction",
              "iam:CreateServiceLinkedRole"
            ],
            "Resource": "*"
        }
    ]
}
```

## Additional Required IAM Permissions<a name="application-auto-scaling-additional-permissions"></a>

Users must have additional permissions for each type of resource for which they will configure scaling policies\. You specify the following actions in the `Action` element of an IAM policy statement\. 

**AppStream 2\.0 fleets**
+ `appstream:DescribeFleets`
+ `appstream:UpdateFleet`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Amazon Comprehend document classification endpoints**
+ `comprehend:UpdateEndpoint`
+ `comprehend:DescribeEndpoint`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**DynamoDB tables and global secondary indexes**
+ `dynamodb:DescribeTable`
+ `dynamodb:UpdateTable`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Amazon EC2 Spot Fleet requests**
+ `ec2:DescribeSpotFleetRequests`
+ `ec2:ModifySpotFleetRequest`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Amazon ECS services**
+ `ecs:DescribeServices`
+ `ecs:UpdateServices`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Amazon EMR clusters**
+ `elasticmapreduce:ModifyInstanceGroups`
+ `elasticmapreduce:ListInstanceGroups`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Aurora DB clusters**
+ `rds:AddTagsToResource`
+ `rds:CreateDBInstance`
+ `rds:DeleteDBInstance`
+ `rds:DescribeDBClusters`
+ `rds:DescribeDBInstances`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Amazon SageMaker endpoints**
+ `sagemaker:DescribeEndpoint`
+ `sagemaker:DescribeEndpointConfig`
+ `sagemaker:UpdateEndpointWeightsAndCapacities`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**AWS Lambda functions**
+ `lambda:PutProvisionedConcurrencyConfig`
+ `lambda:GetProvisionedConcurrencyConfig`
+ `lambda:DeleteProvisionedConcurrencyConfig`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

**Custom Resources**
+ `execute-api:Invoke`
+ `cloudwatch:DeleteAlarms `
+ `cloudwatch:DescribeAlarms `
+ `cloudwatch:PutMetricAlarm `

For services that support step scaling, users may require additional permissions to create and manage CloudWatch alarms\. For more information, see [Permissions Required to Use the CloudWatch Console](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-identity-based-access-control-cw.html#console-permissions-cw) in the *Amazon CloudWatch User Guide*\. 