# Step Scaling Policies for Application Auto Scaling<a name="application-auto-scaling-step-scaling-policies"></a>

Step scaling policies increase or decrease the current capacity of a scalable target based on a set of scaling adjustments, known as *step adjustments*, that vary based on the size of the alarm breach\.

After a scaling activity is started, the policy continues to respond to additional alarms, even while a scaling activity or health check replacement is in progress\. Therefore, all alarms that are breached are evaluated by Application Auto Scaling as it receives the alarm messages\.

**Limits**
+ You cannot create step scaling policies for DynamoDB tables and global secondary indexes\.
+ You can create 50 scaling policies per scalable target\. This includes both step scaling policies and target tracking policies\.
+ You can create 20 step adjustments per scaling policy\.

## Scaling Adjustment Types<a name="as-scaling-adjustment"></a>

When a step scaling policy is performed, it changes the current capacity of your scalable target using the scaling adjustment specified in the policy\. A scaling adjustment can't change the capacity of the scalable target above the maximum capacity or below the minimum capacity\.

Application Auto Scaling supports the following adjustment types for step scaling policies:
+ **ChangeInCapacity**—Increase or decrease the current capacity of the scalable target by the specified value\. A positive value increases the capacity and a negative value decreases the capacity\.

  Example: If the current capacity of a Spot Fleet is 3 and the adjustment is 5, then when this policy is performed, Application Auto Scaling adds 5 to the capacity of the Spot Fleet for a total of 8\.
+ **ExactCapacity**—Change the current capacity of the scalable target to the specified value\. Specify a positive value with this adjustment type\.

  Example: If the current capacity of a Spot Fleet is 3 and the adjustment is 5, then when this policy is performed, Application Auto Scaling changes the capacity to 5\.
+ **PercentChangeInCapacity**—Increase or decrease the current capacity of the scalable target by the specified percentage\. A positive value increases the capacity and a negative value decreases the capacity\. If the resulting value is not an integer, Application Auto Scaling rounds it as follows:
  + Values greater than 1 are rounded down\. For example, `12.7` is rounded to `12`\.
  + Values between 0 and 1 are rounded to 1\. For example, `.67` is rounded to `1`\.
  + Values between 0 and \-1 are rounded to \-1\. For example, `-.58` is rounded to `-1`\.
  + Values less than \-1 are rounded up\. For example, `-6.67` is rounded to `-6`\.

  Example: If the current capacity is 10 and the adjustment is 10 percent, then when this policy is performed, Application Auto Scaling adds 1 to the capacity for a total of 11\.

With **PercentChangeInCapacity**, you can also specify the minimum amount to scale \(using the `MinAdjustmentMagnitude` parameter\. For example, suppose that you create a policy that adds 25 percent and you specify a minimum amount of 2\. If the scalable target has a capacity of 4 and the scaling policy is performed, 25 percent of 4 is 1\. However, because you specified a minimum increment of 2, Application Auto Scaling adds 2\.

## Step Adjustments<a name="as-scaling-steps"></a>

When you create a step scaling policy, you add one or more step adjustments that enable you to scale based on the size of the alarm breach\. Each step adjustment specifies a lower bound for the metric value, an upper bound for the metric value, and the amount by which to scale, based on the scaling adjustment type\.

There are a few rules for the step adjustments for your policy:
+ The ranges of your step adjustments can't overlap or have a gap\.
+ Only one step adjustment can have a null lower bound \(negative infinity\)\. If one step adjustment has a negative lower bound, then there must be a step adjustment with a null lower bound\.
+ Only one step adjustment can have a null upper bound \(positive infinity\)\. If one step adjustment has a positive upper bound, then there must be a step adjustment with a null upper bound\.
+ The upper and lower bound can't be null in the same step adjustment\.
+ If the metric value is above the breach threshold, the lower bound is inclusive and the upper bound is exclusive\. If the metric value is below the breach threshold, the lower bound is exclusive and the upper bound is inclusive\.

Application Auto Scaling applies the aggregation type to the metric data points from all scalable targets\. It compares the aggregated metric value against the upper and lower bounds defined by the step adjustments to determine which step adjustment to perform\.

You specify the upper and lower bounds relative to the breach threshold\. For example, suppose that you have an alarm with a breach threshold of 50 and a scaling adjustment of type `PercentChangeInCapacity`\. You also have scale\-out and scale\-in policies with the following step adjustments:


|  | 
| --- |
|  Scale out policy  | 
| Lower bound | Upper bound | Adjustment | Metric value | 
|  0  |  10  |  0  |  50 <= *value* < 60  | 
|  10  |  20  |  10  |  60 <= *value* < 70  | 
|  20  |  null  |  30  |  70 <= *value* < \+infinity  | 
|   Scale in policy   | 
| Lower bound | Upper bound | Adjustment | Metric value | 
|  \-10  |  0  |  0  |  40 < *value* <= 50  | 
|  \-20  |  \-10  |  \-10  |  30 < *value* <= 40  | 
|  null  |  \-20  |  \-30  |  \-infinity < *value* <= 30  | 

Your scalable target has both a current capacity and a desired capacity of 10\. The current and desired capacity is maintained while the aggregated metric value is greater than 40 and less than 60\.

If the metric value gets to 60, Application Auto Scaling increases the desired capacity of the group by 1, to 11, based on the second step adjustment of the scale\-out policy \(add 10 percent of 10\)\. After the new capacity is added, Application Auto Scaling increases the current capacity to 11\. If the metric value rises to 70 even after this increase in capacity, Application Auto Scaling increases the target capacity by 3, to 14, based on the third step adjustment of the scale\-out policy \(add 30 percent of 11, 3\.3, rounded down to 3\)\.

If the metric value gets to 40, Application Auto Scaling decreases the target capacity by 1, to 13, based on the second step adjustment of the scale\-in policy \(remove 10 percent of 14, 1\.4, rounded down to 1\)\. If the metric value falls to 30 even after this decrease in capacity, Application Auto Scaling decreases the target capacity by 3, to 10, based on the third step adjustment of the scale\-in policy \(remove 30 percent of 13, 3\.9, rounded down to 3\)\.

## Cooldown Period<a name="step-scaling-cooldown"></a>

The *cooldown period* is the amount of time, in seconds, after a scaling activity completes where previous trigger\-related scaling activities can influence future scaling events\.

For scale out policies, while the cooldown period is in effect, the capacity that has been added by the previous scale out event that initiated the cooldown is calculated as part of the desired capacity for the next scale out event\. The intention is to continuously \(but not excessively\) scale out\. For example, when an alarm triggers a step scaling policy to increase the capacity by 2, the scaling activity completes successfully and a cooldown period starts\. During the cooldown period, if the alarm triggers the same policy again but at a more aggressive step adjustment, say by 3, the increase of 2 from the previous scale out event is considered part of the current capacity\. Therefore, only 1 is added to the capacity\.

For scale in policies, the cooldown period is used to block subsequent scale in events until it has expired\. The intention is to scale in conservatively to protect your application's availability\. However, if another alarm triggers a scale out policy during the cooldown period after a scale in event, Application Auto Scaling scales out your scalable target immediately\.

## Create a Step Scaling Policy<a name="create-step-scaling-policy"></a>

You can create scaling policies that tell Application Auto Scaling what to do when the specified conditions change\. Before you can create a scaling policy, you must register the scalable target\.

Use the following [register\-scalable\-target](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a scalable target\.

```
aws application-autoscaling register-scalable-target --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--min-capacity 2 --max-capacity 10
```

The following is an example step configuration with three step adjustments\. Save this configuration in a file named `config.json`\.

```
{
  "AdjustmentType": "ChangeInCapacity",
  "Cooldown": 60,
  "StepAdjustments": [ 
    {
      "MetricIntervalLowerBound": 0,
      "MetricIntervalUpperBound": 10,
      "ScalingAdjustment": 0
    },
    {
      "MetricIntervalLowerBound": 10,
      "MetricIntervalUpperBound": 20,
      "ScalingAdjustment": 1
    },
    {
      "MetricIntervalLowerBound": 20,
      "ScalingAdjustment": 3
    }
  ]
}
```

Use the following [put\-scaling\-policy](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command, along with the `config.json` file you created, to create a scaling policy named `my-step-scaling-policy`:

```
aws application-autoscaling put-scaling-policy --service-namespace ec2 \ 
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--policy-name my-step-scaling-policy --policy-type StepScaling \
--step-scaling-policy-configuration file://config.json
```

## Describe Step Scaling Policies<a name="describe-step-scaling-policy"></a>

You can describe all scaling policies for the specified service namespace using the following [describe\-scaling\-policies](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) command\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2
```

You can filter the results to just the step scaling policies using the `--query` parameter\. Note that this syntax for `query` works on Linux or MacOS\. On Windows, change the single quotes to double quotes\.

```
aws application-autoscaling describe-scaling-policies --service-namespace ec2 \
--query 'ScalingPolicies[?PolicyType==`StepScaling`]'
```

The following is example output\.

```
[
    {
        "PolicyARN": "<arn>",
        "StepScalingPolicyConfiguration": {
            "Cooldown": 60,
            "StepAdjustments": [
                {
                    "MetricIntervalLowerBound": 0.0,
                    "ScalingAdjustment": 0,
                    "MetricIntervalUpperBound": 10.0
                },
                {
                    "MetricIntervalLowerBound": 10.0,
                    "ScalingAdjustment": 10,
                    "MetricIntervalUpperBound": 20.0
                },
                {
                    "MetricIntervalLowerBound": 20.0,
                    "ScalingAdjustment": 30
                }
            ],
            "AdjustmentType": "ChangeInCapacity"
        },
        "PolicyType": "StepScaling",
        "ResourceId": "spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE",
        "ServiceNamespace": "ec2",
        "Alarms": [],
        "PolicyName": "my-step-scaling-policy",
        "ScalableDimension": "ec2:spot-fleet-request:TargetCapacity",
        "CreationTime": 1515024099.901
    }
]
```

## Delete a Step Scaling Policy<a name="delete-step-scaling-policy"></a>

When you are finished with a step tracking scaling policy, you can delete it using the [delete\-scaling\-policy](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command\.

The following command deletes the specified step scaling policy for the specified Spot fleet request\.

```
aws application-autoscaling delete-scaling-policy --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--policy-name my-step-scaling-policy
```