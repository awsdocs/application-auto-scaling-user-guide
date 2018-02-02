# Authentication and Access Control for Application Auto Scaling<a name="auth-and-access-control"></a>

By default, IAM users don't have permission to create or modify AWS resources\. To grant IAM users permission to create or modify AWS resources, you must create policies using AWS Identity and Access Management \(IAM\)\. IAM policies grant permissions to specific resources and API actions\. You attach am IAM policy to the IAM users or groups that require the permissions it grants\. For more information, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

## Application Auto Scaling Actions<a name="application-auto-scaling-actions"></a>

You can specify any and all Application Auto Scaling actions in an IAM policy\. Use the following prefix with the name of the action: `application-autoscaling:`\. For example:

```
"Action": "application-autoscaling:DescribeScalingActivities"
```

You can also use wildcards\. For example, use `application-autoscaling:*` to specify all Auto Scaling actions\.

```
"Action": "application-autoscaling:*"
```

Use `Describe:*` to specify all actions whose names start with `Describe`\.

```
"Action": "application-autoscaling:Describe*"
```

For a list of actions, see [Application Auto Scaling Actions](http://docs.aws.amazon.com/autoscaling/application/APIReference/API_Operations.html) in the *Application Auto Scaling API Reference*\.

## Application Auto Scaling Resources<a name="application-auto-scaling-resources"></a>

When writing an IAM policy to control access to Application Auto Scaling actions, you must use "\*" as the resource\. There are no supported Amazon Resource Names \(ARNs\) for Application Auto Scaling resources\.

## Application Auto Scaling Keys<a name="application-autoscaling-keys"></a>

For a list of context keys supported by each AWS service and a list of AWS\-wide policy keys, see [AWS Service Actions and Condition Context Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actionsconditions.html) and [Global and IAM Condition Context Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

## Example Policies<a name="application-auto-scaling-example-policies"></a>

To configure step scaling policies or target tracking policies for a scalable resource, users must have permission to use the actions in the following example policy\.

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

To configure scheduled scaling for a scalable resource, users must have permission to use the actions in the following example policy\.

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

Users must have the following additional permissions for each type of scalable resource for which they will configure scaling policies\.

**ECS services**

+ `ecs:DescribeServices`

+ `ecs:UpdateServices`

**Spot Fleet requests**

+ `ec2:DescribeSpotFleetRequests`

+ `ec2:ModifySpotFleetRequest`

**DynamoDB tables**

+ `dynamodb:DescribeTable`

+ `dynamodb:UpdateTable`

**Aurora DB clusters**

+ `rds:AddTagsToResource`

+ `rds:CreateDBInstance`

+ `rds:DeleteDBInstance`

+ `rds:DescribeDBClusters`

+ `rds:DescribeDBInstances`