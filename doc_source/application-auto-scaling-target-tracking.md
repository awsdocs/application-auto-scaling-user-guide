# Target tracking scaling policies for Application Auto Scaling<a name="application-auto-scaling-target-tracking"></a>

With target tracking scaling policies, you choose a scaling metric and set a target value\. Application Auto Scaling creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value\. The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value\. In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern\.

To ensure application availability, Application Auto Scaling scales out proportionally to the metric as fast as it can, but scales in more gradually\.

**Limits**
+ Target tracking scaling policies are not supported for Amazon EMR\.
+ Scale in is disabled for Amazon MSK\.
+ You cannot use the `RegisterScalableTarget` or `PutScalingPolicy` API operations to update an AWS Auto Scaling scaling plan\. For information about the AWS Auto Scaling console, CLI, or API actions, see the [AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/index.html#lang/en_us) documentation\.
+ You can use Application Auto Scaling to apply a target tracking scaling policy based on a predefined or CloudWatch custom metric\. Not all services allow you to manage custom metrics through the console, however\. To see if a service supports custom metrics in the console, consult the documentation for that service\.

## Considerations<a name="target-tracking-considerations"></a>

Keep the following considerations in mind:
+ Not all metrics work for target tracking\. This can be important when you are specifying a custom metric\. The metric must be a valid utilization metric and describe how busy a scalable target is\. The metric value must increase or decrease proportionally to the capacity of the scalable target so that the metric data can be used to proportionally scale the scalable target\.
+ Wherever possible, you should scale on metrics with a 1\-minute frequency because that ensures a faster response to utilization changes\.
+ A target tracking scaling policy assumes that it should perform scale out when the specified metric is above the target value\. You cannot use a target tracking scaling policy to scale out when the specified metric is below the target value\.
+ A target tracking scaling policy does not perform scaling when the specified metric has insufficient data, for example, because of a network connectivity issue\. It does not perform scale in because it does not interpret insufficient data as low utilization\. 
+ You may see gaps between the target value and the actual metric data points\. This is because Application Auto Scaling always acts conservatively by rounding up or down when it determines how much capacity to add or remove\. This prevents it from adding insufficient capacity or removing too much capacity\. However, for a scalable target with a small capacity, the actual metric data points might seem far from the target value\. 
+ For a scalable target with a larger capacity, adding or removing capacity causes less of a gap between the target value and the actual metric data points\.
+ You can have multiple target tracking scaling policies for a scalable target, provided that each of them uses a different metric\. The intention of Application Auto Scaling is to always prioritize availability, so its behavior differs depending on whether the target tracking policies are ready for scale out or scale in\. It will scale out the scalable target if any of the target tracking policies are ready for scale out, but will scale in only if all of the target tracking policies \(with the scale\-in portion enabled\) are ready to scale in\. 
+ If multiple policies instruct the scalable target to scale out or in at the same time, Application Auto Scaling scales based on the policy that provides the largest capacity for both scale in and scale out\. This provides greater flexibility to cover multiple scenarios and ensures that there is always enough capacity to process your application workloads\. 
+ You can disable the scale\-in portion of a target tracking scaling policy\. This feature provides the flexibility to use a different method for scale in than you use for scale out\. For example, you can use a step scaling policy for scale in while using a target tracking scaling policy for scale out\. 
+ We recommend caution, however, when using target tracking scaling policies with step scaling policies because conflicts between these policies can cause undesirable behavior\. For example, if the step scaling policy initiates a scale\-in activity before the target tracking policy is ready to scale in, the scale\-in activity will not be blocked\. After the scale\-in activity completes, the target tracking policy could instruct the scalable target to scale out again\. 
+ Do not edit or delete the CloudWatch alarms that are configured for the target tracking scaling policy\. CloudWatch alarms that are associated with your target tracking scaling policies are managed by AWS and deleted automatically when no longer needed\. 

## Cooldown period<a name="target-tracking-cooldown"></a>

The amount of time to wait for a previous scaling activity to take effect is called the cooldown period\. 

With target tracking scaling policies, there are two types of cooldown periods:
+ With the *scale\-out cooldown period*, the intention is to continuously \(but not excessively\) scale out\. After Application Auto Scaling successfully scales out using a target tracking scaling policy, it starts to calculate the cooldown time\. The scaling policy won't increase the desired capacity again unless either a larger scale out is triggered or the cooldown period ends\. While the scale\-out cooldown period is in effect, the capacity added by the initiating scale\-out activity is calculated as part of the desired capacity for the next scale\-out activity\.
+ With the *scale\-in cooldown period*, the intention is to scale in conservatively to protect your application's availability, so scale\-in activities are blocked until the cooldown period has expired\. However, if another alarm triggers a scale\-out activity during the scale\-in cooldown period, Application Auto Scaling scales out the target immediately\. In this case, the scale\-in cooldown period stops and doesn't complete\.

Each cooldown period is measured in seconds and applies only to scaling policy\-related scaling activities\. During a cooldown period, when a scheduled action starts at the scheduled time, it can trigger a scaling activity immediately without waiting for the cooldown period to expire\.

You can start with the default values, which can be later fine\-tuned\. For example, you might need to increase a cooldown period to prevent your target tracking scaling policy from being too aggressive about changes that occur over short periods of time\. For the default values, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\.

## Supporting application availability during high utilization periods<a name="target-tracking-high-utilization"></a>

A target tracking scaling policy is more aggressive in adding capacity when utilization increases than it is in removing capacity when utilization decreases\. For example, if the policy's specified metric reaches its target value, the policy assumes that your application is already heavily loaded\. So it responds by adding capacity proportional to the metric value as fast as it can\. The higher the metric, the more capacity is added\.

When the metric falls below the target value, the policy expects that utilization will eventually increase again\. So it slows down scaling by removing capacity only when utilization passes a threshold that is far enough below the target value \(usually 20% lower\) for utilization to be considered to have slowed\. The intention of this more conservative behavior is to ensure that removing capacity only happens when the application is no longer experiencing demand at the same high level that it was previously\. This is currently the default behavior for all target tracking scaling policies \(though the behavior could change in the future\)\.

For workloads that are cyclical in nature, you also have the option to automate capacity changes on a schedule using scheduled scaling\. For each scheduled action, a new minimum capacity value and a new maximum capacity value can be defined\. These values form the boundaries of the scaling policy\. 

The combination of scheduled scaling and target tracking scaling can help reduce the impact of a sharp increase in utilization levels, when capacity is needed immediately\.

## Register scalable target<a name="target-tracking-register-scalable-target"></a>

Before you can create a scaling policy, you must register the scalable target\. Use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a new scalable target\. 

The following example registers a Spot Fleet request with Application Auto Scaling\. Application Auto Scaling can scale the number of instances in the Spot Fleet at a minimum of 2 instances and a maximum of 10\. 

**Linux, macOS, or Unix**

```
aws application-autoscaling register-scalable-target --service-namespace ec2 \
  --scalable-dimension ec2:spot-fleet-request:TargetCapacity \
  --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
  --min-capacity 2 --max-capacity 10
```

**Windows**

```
aws application-autoscaling register-scalable-target --service-namespace ec2 --scalable-dimension ec2:spot-fleet-request:TargetCapacity --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE --min-capacity 2 --max-capacity 10
```

**Note**  
When you configure scaling policies in the console of the target service, this automatically registers the resource as a scalable target with Application Auto Scaling\.   
For brevity, the examples in this topic illustrate CLI commands for an Amazon EC2 Spot Fleet\. To specify a different scalable target, specify its namespace in `--service-namespace`, its scalable dimension in `--scalable-dimension`, and its resource ID in `--resource-id`\. For a list of valid values for each option, see [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html)\.

## Create a target tracking scaling policy<a name="create-target-tracking-policy"></a>

You can create a target tracking scaling policy that instructs Application Auto Scaling to increase \(scale out\) and decrease \(scale in\) capacity automatically when the load on the application changes\.

**Example: target tracking configuration file**  
The following is an example target tracking configuration that keeps the average CPU utilization at 40 percent\. Save this configuration in a file named `config.json`\.

```
{
  "TargetValue": 40.0,
  "PredefinedMetricSpecification": 
    {
      "PredefinedMetricType": "EC2SpotFleetRequestAverageCPUUtilization"
    }
}
```

For more information, see [PredefinedMetricSpecification](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_PredefinedMetricSpecification.html) in the *Application Auto Scaling API Reference*\.

Alternatively, you can use a custom metric for scaling by creating a customized metric specification and adding values for each parameter from CloudWatch\. The following is an example target tracking configuration that keeps the average utilization of the specified metric at 40 percent\.

```
{
   "TargetValue":40.0,
   "CustomizedMetricSpecification":{
      "MetricName":"MyUtilizationMetric",
      "Namespace":"MyNamespace",
      "Dimensions":[
         {
            "Name":"MyOptionalMetricDimensionName",
            "Value":"MyOptionalMetricDimensionValue"
         }
      ],
      "Statistic":"Average",
      "Unit":"Percent"
   }
}
```

For more information, see [CustomizedMetricSpecification](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_CustomizedMetricSpecification.html) in the *Application Auto Scaling API Reference*\.

**Example: cpu40\-target\-tracking\-scaling\-policy**  
Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command, along with the `config.json` file you created, to create a scaling policy named `cpu40-target-tracking-scaling-policy`\.

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scaling-policy --service-namespace ec2 \
  --scalable-dimension ec2:spot-fleet-request:TargetCapacity \
  --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
  --policy-name cpu40-target-tracking-scaling-policy --policy-type TargetTrackingScaling \
  --target-tracking-scaling-policy-configuration file://config.json
```

**Windows**

```
aws application-autoscaling put-scaling-policy --service-namespace ec2 --scalable-dimension ec2:spot-fleet-request:TargetCapacity --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE --policy-name cpu40-target-tracking-scaling-policy --policy-type TargetTrackingScaling --target-tracking-scaling-policy-configuration file://config.json
```

If successful, this command returns the ARNs and names of the two CloudWatch alarms created on your behalf\.

```
{
    "PolicyARN": "arn:aws:autoscaling:region:account-id:scalingPolicy:policy-id:resource/ec2/spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE:policyName/cpu40-target-tracking-scaling-policy",
    "Alarms": [
        {
            "AlarmARN": "arn:aws:cloudwatch:region:account-id:alarm:TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmHigh-d4f0770c-b46e-434a-a60f-3b36d653feca",
            "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmHigh-d4f0770c-b46e-434a-a60f-3b36d653feca"
        },
        {
            "AlarmARN": "arn:aws:cloudwatch:region:account-id:alarm:TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmLow-1b437334-d19b-4a63-a812-6c67aaf2910d",
            "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmLow-1b437334-d19b-4a63-a812-6c67aaf2910d"
        }
    ]
}
```

## Describe target tracking scaling policies<a name="describe-target-tracking-policy"></a>

You can describe all scaling policies for the specified service namespace using the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) command\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2
```

You can filter the results to just the target tracking scaling policies using the `--query` parameter\. For more information about the syntax for `query`, see [Controlling command output from the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-output.html) in the *AWS Command Line Interface User Guide*\.

**Linux, macOS, or Unix**

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2 \
  --query 'ScalingPolicies[?PolicyType==`TargetTrackingScaling`]'
```

**Windows**

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2 --query "ScalingPolicies[?PolicyType==`TargetTrackingScaling`]"
```

The following is example output\.

```
[
    {
        "PolicyARN": "PolicyARN",
        "TargetTrackingScalingPolicyConfiguration": {
            "PredefinedMetricSpecification": {
                "PredefinedMetricType": "EC2SpotFleetRequestAverageCPUUtilization"
            },
            "TargetValue": 40.0
        },
        "PolicyName": "cpu40-target-tracking-scaling-policy",
        "ScalableDimension": "ec2:spot-fleet-request:TargetCapacity",
        "ServiceNamespace": "ec2",
        "PolicyType": "TargetTrackingScaling",
        "ResourceId": "spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE",
        "Alarms": [
            {
                "AlarmARN": "arn:aws:cloudwatch:region:account-id:alarm:TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmHigh-d4f0770c-b46e-434a-a60f-3b36d653feca",
                "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmHigh-d4f0770c-b46e-434a-a60f-3b36d653feca"
            },
            {
                "AlarmARN": "arn:aws:cloudwatch:region:account-id:alarm:TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmLow-1b437334-d19b-4a63-a812-6c67aaf2910d",
                "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmLow-1b437334-d19b-4a63-a812-6c67aaf2910d"
            }
        ],
        "CreationTime": 1515021724.807
    }
]
```

## Delete a target tracking scaling policy<a name="delete-target-tracking-policy"></a>

When you are finished with a target tracking scaling policy, you can delete it using the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command\.

The following command deletes the specified target tracking scaling policy for the specified Spot Fleet request\. It also deletes the CloudWatch alarms that Application Auto Scaling created on your behalf\.

**Linux, macOS, or Unix**

```
aws application-autoscaling delete-scaling-policy --service-namespace ec2 \
  --scalable-dimension ec2:spot-fleet-request:TargetCapacity \
  --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
  --policy-name cpu40-target-tracking-scaling-policy
```

**Windows**

```
aws application-autoscaling delete-scaling-policy --service-namespace ec2 --scalable-dimension ec2:spot-fleet-request:TargetCapacity --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE --policy-name cpu40-target-tracking-scaling-policy
```