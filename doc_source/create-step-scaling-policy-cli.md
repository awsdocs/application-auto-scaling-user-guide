# Creating a step scaling policy using the AWS CLI<a name="create-step-scaling-policy-cli"></a>

You can create a step scaling policy for Application Auto Scaling by using the AWS CLI for the following configuration tasks\.

**Tasks**
+ [Register a scalable target](#step-scaling-register-scalable-target)
+ [Create a step scaling policy](#create-step-scaling-policy)
+ [Create an alarm that triggers the scaling policy](#step-scaling-create-alarm)

## Register a scalable target<a name="step-scaling-register-scalable-target"></a>

If you haven't already done so, register the scalable target\. Use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a specific resource in the target service as a scalable target\. The following example registers an Amazon ECS service with Application Auto Scaling\. Application Auto Scaling can scale the number of tasks at a minimum of 2 tasks and a maximum of 10\. 

**Linux, macOS, or Unix**

```
aws application-autoscaling register-scalable-target --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/sample-app-service \
  --min-capacity 2 --max-capacity 10
```

**Windows**

```
aws application-autoscaling register-scalable-target --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/sample-app-service --min-capacity 2 --max-capacity 10
```

**Note**  
When you configure scaling policies in the console of the target service, this automatically registers the resource as a scalable target with Application Auto Scaling\.  
For brevity, the examples in this topic illustrate CLI commands for an Amazon ECS service\. To specify a different scalable target, specify its namespace in `--service-namespace`, its scalable dimension in `--scalable-dimension`, and its resource ID in `--resource-id`\. For a list of valid values for each option, see [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html)\.

## Create a step scaling policy<a name="create-step-scaling-policy"></a>

The following is an example step configuration with an adjustment type of `ChangeInCapacity` that increases the capacity of the scalable target based on the following step adjustments \(assuming a CloudWatch alarm threshold of 70 percent\): 
+ Increase capacity by 1 when the value of the metric is greater than or equal to 70 percent but less than 85 percent 
+ Increase capacity by 2 when the value of the metric is greater than or equal to 85 percent but less than 95 percent 
+ Increase capacity by 3 when the value of the metric is greater than or equal to 95 percent 

Save this configuration in a file named `config.json`\.

```
{
  "AdjustmentType": "ChangeInCapacity",
  "MetricAggregationType": "Average",
  "Cooldown": 60,
  "StepAdjustments": [ 
    {
      "MetricIntervalLowerBound": 0,
      "MetricIntervalUpperBound": 15,
      "ScalingAdjustment": 1
    },
    {
      "MetricIntervalLowerBound": 15,
      "MetricIntervalUpperBound": 25,
      "ScalingAdjustment": 2
    },
    {
      "MetricIntervalLowerBound": 25,
      "ScalingAdjustment": 3
    }
  ]
}
```

Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command, along with the `config.json` file that you created, to create a scaling policy named `my-step-scaling-policy`\.

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scaling-policy --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/sample-app-service \
  --policy-name my-step-scaling-policy --policy-type StepScaling \
  --step-scaling-policy-configuration file://config.json
```

**Windows**

```
aws application-autoscaling put-scaling-policy --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/sample-app-service --policy-name my-step-scaling-policy --policy-type StepScaling --step-scaling-policy-configuration file://config.json
```

The output includes the ARN that serves as a unique name for the policy\. You need it to create CloudWatch alarms\.

```
{
    "PolicyARN": "arn:aws:autoscaling:region:123456789012:scalingPolicy:ac542982-cbeb-4294-891c-a5a941dfa787:resource/ecs/service/default/sample-app-service:policyName/my-step-scaling-policy"
}
```

## Create an alarm that triggers the scaling policy<a name="step-scaling-create-alarm"></a>

Finally, use the following CloudWatch [https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/put-metric-alarm.html) command to create an alarm to use with your step scaling policy\. In this example, you have an alarm based on average CPU utilization\. The alarm is configured to be in an ALARM state if it reaches a threshold of 70 percent for at least two consecutive evaluation periods of 60 seconds\. To specify a different CloudWatch metric or use your own custom metric, specify its name in `--metric-name` and its namespace in `--namespace`\. 

**Linux, macOS, or Unix**

```
aws cloudwatch put-metric-alarm --alarm-name Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service \
  --metric-name CPUUtilization --namespace AWS/ECS --statistic Average \
  --period 60 --evaluation-periods 2 --threshold 70 \
  --comparison-operator GreaterThanOrEqualToThreshold \
  --dimensions Name=ClusterName,Value=default Name=ServiceName,Value=sample-app-service \
  --alarm-actions PolicyARN
```

**Windows**

```
aws cloudwatch put-metric-alarm --alarm-name Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service --metric-name CPUUtilization --namespace AWS/ECS --statistic Average --period 60 --evaluation-periods 2 --threshold 70 --comparison-operator GreaterThanOrEqualToThreshold --dimensions Name=ClusterName,Value=default Name=ServiceName,Value=sample-app-service --alarm-actions PolicyARN
```

## Describe step scaling policies<a name="describe-step-scaling-policy"></a>

You can describe all scaling policies for the specified service namespace using the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) command\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ecs
```

You can filter the results to just the step scaling policies using the `--query` parameter\. For more information about the syntax for `query`, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

**Linux, macOS, or Unix**

```
aws application-autoscaling describe-scaling-policies --service-namespace ecs \
  --query 'ScalingPolicies[?PolicyType==`StepScaling`]'
```

**Windows**

```
aws application-autoscaling describe-scaling-policies --service-namespace ecs --query "ScalingPolicies[?PolicyType==`StepScaling`]"
```

The following is example output\.

```
[
    {
        "PolicyARN": "PolicyARN",
        "StepScalingPolicyConfiguration": {
            "MetricAggregationType": "Average",
            "Cooldown": 60,
            "StepAdjustments": [
                {
                    "MetricIntervalLowerBound": 0.0,
                    "MetricIntervalUpperBound": 15.0,
                    "ScalingAdjustment": 1
                },
                {
                    "MetricIntervalLowerBound": 15.0,
                    "MetricIntervalUpperBound": 25.0,
                    "ScalingAdjustment": 2
                },
                {
                    "MetricIntervalLowerBound": 25.0,
                    "ScalingAdjustment": 3
                }
            ],
            "AdjustmentType": "ChangeInCapacity"
        },
        "PolicyType": "StepScaling",
        "ResourceId": "service/default/sample-app-service",
        "ServiceNamespace": "ecs",
        "Alarms": [
            {
                "AlarmName": "Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service",
                "AlarmARN": "arn:aws:cloudwatch:region:012345678910:alarm:Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service"
            }
        ],
        "PolicyName": "my-step-scaling-policy",
        "ScalableDimension": "ecs:service:DesiredCount",
        "CreationTime": 1515024099.901
    }
]
```

## Delete a step scaling policy<a name="delete-step-scaling-policy"></a>

When you no longer need a step scaling policy, you can delete it\. To delete both the scaling policy and the CloudWatch alarm, complete the following tasks\. 

**To delete your scaling policy**  
Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command\.

**Linux, macOS, or Unix**

```
aws application-autoscaling delete-scaling-policy --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/sample-app-service \
  --policy-name my-step-scaling-policy
```

**Windows**

```
aws application-autoscaling delete-scaling-policy --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/sample-app-service --policy-name my-step-scaling-policy
```

**To delete the CloudWatch alarm**  
Use the [https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/delete-alarms.html](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/delete-alarms.html) command\. You can delete one or more alarms at a time\. For example, use the following command to delete the `Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service` and `Step-Scaling-AlarmLow-ECS:service/default/sample-app-service` alarms\.

```
aws cloudwatch delete-alarms --alarm-name Step-Scaling-AlarmHigh-ECS:service/default/sample-app-service Step-Scaling-AlarmLow-ECS:service/default/sample-app-service
```