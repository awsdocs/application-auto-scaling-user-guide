# Target Tracking Scaling Policies for Application Auto Scaling<a name="application-auto-scaling-target-tracking"></a>

With target tracking scaling policies, you select a predefined metric or configure a customized metric, and set a target value\. Application Auto Scaling creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value\. The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value\. In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern and minimizes changes to the capacity of the scalable target\.

When specifying a customized metric, be aware that not all metrics work for target tracking\. The metric must be a valid utilization metric and describe how busy a scalable target is\. The metric value must increase or decrease proportionally to the capacity of the scalable target so that the metric data can be used to proportionally scale the scalable target\.

You can have multiple target tracking scaling policies for a scalable target, provided that each of them uses a different metric\. Application Auto Scaling scales based on the policy that provides the largest capacity for both scale in and scale out\. This provides greater flexibility to cover multiple scenarios and ensures that there is always enough capacity to process your application workloads\.

You can also optionally disable the scale\-in portion of a target tracking scaling policy\. This feature provides the flexibility to use a different method for scale in than you use for scale out \(for example, a different scaling policy type\)\.

**Limits**
+ You cannot create target tracking scaling policies for Amazon EMR clusters or AppStream 2\.0 fleets\.
+ You can create 50 scaling policies per scalable target\. This includes both step scaling policies and target tracking policies\.

## Considerations<a name="target-tracking-considerations"></a>

Keep the following considerations in mind:
+ A target tracking scaling policy assumes that it should perform scale out when the specified metric is above the target value\. You cannot use a target tracking scaling policy to scale out when the specified metric is below the target value\.
+ A target tracking scaling policy does not perform scaling when the specified metric has insufficient data\. It does not perform scale in because it does not interpret insufficient data as low utilization\. To scale in when a metric has insufficient data, create a step scaling policy and have an alarm invoke the scaling policy when it changes to the `INSUFFICIENT_DATA` state\. For information about alarm states, see [Alarm States](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html#alarm-states) in the *Amazon CloudWatch User Guide*\. 
+ You may see gaps between the target value and the actual metric data points\. This is because Application Auto Scaling always acts conservatively by rounding up or down when it determines how much capacity to add or remove\. This prevents it from adding insufficient capacity or removing too much capacity\. However, for a scalable target with small capacity, the actual metric data points might seem far from the target value\. For a scalable target with larger capacity, adding or removing capacity causes less of a gap between the target value and the actual metric data points\.
+ We recommend that you scale based on metrics with a 1\-minute frequency because that ensures a faster response to utilization changes\. Scaling on metrics with a 5\-minute frequency can result in slower response time and scaling on stale metric data\.
+ To ensure application availability, Application Auto Scaling scales out proportionally to the metric as fast as it can, but scales in more gradually\.
+ Do not edit or delete the CloudWatch alarms that Application Auto Scaling manages for a target tracking scaling policy\. Application Auto Scaling deletes the alarms automatically when you delete the Auto Scaling policy\.

## Cooldown Period<a name="target-tracking-cooldown"></a>

The *scale out cooldown period* is the amount of time, in seconds, after a scale out activity completes before another scale out activity can start\. While this cooldown period is in effect, the capacity that has been added by the previous scale out event that initiated the cooldown is calculated as part of the desired capacity for the next scale out event\. The intention is to continuously \(but not excessively\) scale out\.

The *scale in cooldown period* is the amount of time, in seconds, after a scale in activity completes before another scale in activity can start\. This cooldown period is used to block subsequent scale in events until it has expired\. The intention is to scale in conservatively to protect your application's availability\. However, if another alarm triggers a scale out policy during the cooldown period after a scale in event, Application Auto Scaling scales out your scalable target immediately\.

## Create a Target Tracking Scaling Policy<a name="create-target-tracking-policy"></a>

You can create scaling policies that tell Application Auto Scaling what to do when the specified conditions change\. Before you can create a scaling policy, you must register a scalable target\.

Use the following [register\-scalable\-target](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a scalable target\.

```
aws application-autoscaling register-scalable-target --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--min-capacity 2 --max-capacity 10
```

The following is an example target tracking configuration that keeps average CPU utilization at 40 percent\. Save this configuration in a file named `config.json`\.

```
{
  "TargetValue": 40.0,
  "PredefinedMetricSpecification": 
    {
      "PredefinedMetricType": "EC2SpotFleetRequestAverageCPUUtilization"
    }
}
```

Use the following [put\-scaling\-policy](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command, along with the `config.json` file you created, to create a scaling policy named `cpu40` that keeps the average CPU utilization of the specified scalable target at 40 percent:

```
aws application-autoscaling put-scaling-policy --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--policy-name cpu40 --policy-type TargetTrackingScaling \
--target-tracking-scaling-policy-configuration file://config.json
```

The following is example output\. It contains the ARNs and names of the two CloudWatch alarms created on your behalf\.

```
{
    "PolicyARN": "arn:aws:autoscaling:region:account-id:scalingPolicy:policy-id:resource/ec2/spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE:policyName/cpu40",
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

## Describe Target Tracking Scaling Policies<a name="describe-target-tracking-policy"></a>

You can describe all scaling policies for the specified service namespace using the following [describe\-scaling\-policies](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) command\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2
```

You can filter the results to just the target tracking scaling policies using the `--query` parameter\. Note that this syntax for `query` works on Linux or MacOS\. On Windows, change the single quotes to double quotes\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2 \
--query 'ScalingPolicies[?PolicyType==`TargetTrackingScaling`]'
```

The following is example output\.

```
[
    {
        "PolicyARN": "<arn>",
        "TargetTrackingScalingPolicyConfiguration": {
            "PredefinedMetricSpecification": {
                "PredefinedMetricType": "EC2SpotFleetRequestAverageCPUUtilization"
            },
            "TargetValue": 40.0
        },
        "PolicyName": "cpu40",
        "ScalableDimension": "ec2:spot-fleet-request:TargetCapacity",
        "ServiceNamespace": "ec2",
        "PolicyType": "TargetTrackingScaling",
        "ResourceId": "spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE",
        "Alarms": [
            {
                "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmHigh-d4f0770c-b46e-434a-a60f-3b36d653feca",
                "AlarmARN": "<arn>"
            },
            {
                "AlarmName": "TargetTracking-spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE-AlarmLow-1b437334-d19b-4a63-a812-6c67aaf2910d",
                "AlarmARN": "<arn>"
            }
        ],
        "CreationTime": 1515021724.807
    }
]
```

## Delete a Target Tracking Scaling Policy<a name="delete-target-tracking-policy"></a>

When you are finished with a target tracking scaling policy, you can delete it using the [delete\-scaling\-policy](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command\.

The following command deletes the specified target tracking scaling policy for the specified Spot fleet request\. It also deletes the CloudWatch alarms that Application Auto Scaling created on your behalf\.

```
aws application-autoscaling delete-scaling-policy --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--policy-name cpu40
```