# Step scaling policies for Application Auto Scaling<a name="application-auto-scaling-step-scaling-policies"></a>

With step scaling, you choose scaling metrics and threshold values for the CloudWatch alarms that trigger the scaling process as well as define how your scalable target should be scaled when a threshold is in breach for a specified number of evaluation periods\. 

If your scaling metric is a utilization metric that increases or decreases proportionally to the capacity of the scalable target, we recommend that you use a target tracking scaling policy\. For more information, see [Target tracking scaling policies for Application Auto Scaling](application-auto-scaling-target-tracking.md)\. You still have the option to use target tracking scaling with step scaling for a more advanced scaling policy configuration\. For example, if you want, you can configure a more aggressive response when utilization reaches a certain level\. 

Step scaling policies increase or decrease the current capacity of a scalable target based on a set of scaling adjustments, known as *step adjustments*\. The adjustments vary based on the size of the alarm breach\. All alarms that are breached are evaluated by Application Auto Scaling as it receives the alarm messages\.

## Step adjustments<a name="as-scaling-steps"></a>

When you create a step scaling policy, you add one or more step adjustments that enable you to scale based on the size of the alarm breach\. Each step adjustment specifies the following:
+ A lower bound for the metric value
+ An upper bound for the metric value
+ The amount by which to scale, based on the scaling adjustment type

CloudWatch aggregates metric data points based on the statistic for the metric associated with your CloudWatch alarm\. When the alarm is breached, the appropriate scaling policy is triggered\. Application Auto Scaling applies your specified aggregation type to the most recent metric data points from CloudWatch \(as opposed to the raw metric data\)\. It compares this aggregated metric value against the upper and lower bounds defined by the step adjustments to determine which step adjustment to perform\. 

You specify the upper and lower bounds relative to the breach threshold\. For example, let's say that you have a scalable target that has both a current capacity and a desired capacity of 10\. You have a CloudWatch alarm with a breach threshold of 50 percent\. You have an adjustment type of `PercentChangeInCapacity` and scale\-out and scale\-in policies with the following step adjustments:


**Example: Step adjustments for scale\-out policy**  

| **Lower bound** | **Upper bound** | **Adjustment** | 
| --- | --- | --- | 
|  0  |  10  |  0  | 
|  10  |  20  |  10  | 
|  20  |  null  |  30  | 


**Example: Step adjustments for scale\-in policy**  

| **Lower bound** | **Upper bound** | **Adjustment** | 
| --- | --- | --- | 
|  \-10  |  0  |  0  | 
|  \-20  |  \-10  |  \-10  | 
|  null  |  \-20  |  \-30  | 

This creates the following scaling configuration\.

```
Metric value

-infinity          30%    40%          60%     70%             infinity
-----------------------------------------------------------------------
          -30%      | -10% | Unchanged  | +10%  |       +30%        
-----------------------------------------------------------------------
```

The following points summarize the behavior of the scaling configuration in relation to the desired and current capacity of the scalable target:
+ The current and desired capacity is maintained while the aggregated metric value is greater than 40 and less than 60\.
+ If the metric value gets to 60, Application Auto Scaling increases the desired capacity of the scalable target by 1, to 11\. That's based on the second step adjustment of the scale\-out policy \(add 10 percent of 10\)\. After the new capacity is added, Application Auto Scaling increases the current capacity to 11\. If the metric value rises to 70 even after this increase in capacity, Application Auto Scaling increases the target capacity by 3, to 14\. That's based on the third step adjustment of the scale\-out policy \(add 30 percent of 11, 3\.3, rounded down to 3\)\.
+ If the metric value gets to 40, Application Auto Scaling decreases the target capacity by 1, to 13, based on the second step adjustment of the scale\-in policy \(remove 10 percent of 14, 1\.4, rounded down to 1\)\. If the metric value falls to 30 even after this decrease in capacity, Application Auto Scaling decreases the target capacity by 3, to 10, based on the third step adjustment of the scale\-in policy \(remove 30 percent of 13, 3\.9, rounded down to 3\)\.

When you specify the step adjustments for your scaling policy, note the following:
+ The ranges of your step adjustments can't overlap or have a gap\.
+ Only one step adjustment can have a null lower bound \(negative infinity\)\. If one step adjustment has a negative lower bound, then there must be a step adjustment with a null lower bound\.
+ Only one step adjustment can have a null upper bound \(positive infinity\)\. If one step adjustment has a positive upper bound, then there must be a step adjustment with a null upper bound\.
+ The upper and lower bound can't be null in the same step adjustment\.
+ If the metric value is above the breach threshold, the lower bound is inclusive and the upper bound is exclusive\. If the metric value is below the breach threshold, the lower bound is exclusive and the upper bound is inclusive\.

## Scaling adjustment types<a name="as-scaling-adjustment"></a>

You can define a scaling policy that performs the optimal scaling action, based on the scaling adjustment type that you choose\. You can specify the adjustment type as a percentage of the current capacity of your scalable target or in absolute numbers\.

Application Auto Scaling supports the following adjustment types for step scaling policies:
+ **ChangeInCapacity**—Increase or decrease the current capacity of the scalable target by the specified value\. A positive value increases the capacity and a negative value decreases the capacity\. For example: If the current capacity is 3 and the adjustment is 5, then Application Auto Scaling adds 5 to the capacity for a total of 8\.
+ **ExactCapacity**—Change the current capacity of the scalable target to the specified value\. Specify a positive value with this adjustment type\. For example: If the current capacity is 3 and the adjustment is 5, then Application Auto Scaling changes the capacity to 5\.
+ **PercentChangeInCapacity**—Increase or decrease the current capacity of the scalable target by the specified percentage\. A positive value increases the capacity and a negative value decreases the capacity\. For example: If the current capacity is 10 and the adjustment is 10 percent, then Application Auto Scaling adds 1 to the capacity for a total of 11\. 
**Note**  
If the resulting value is not an integer, Application Auto Scaling rounds it as follows:  
Values greater than 1 are rounded down\. For example, `12.7` is rounded to `12`\.
Values between 0 and 1 are rounded to 1\. For example, `.67` is rounded to `1`\.
Values between 0 and \-1 are rounded to \-1\. For example, `-.58` is rounded to `-1`\.
Values less than \-1 are rounded up\. For example, `-6.67` is rounded to `-6`\.

  With **PercentChangeInCapacity**, you can also specify the minimum amount to scale using the `MinAdjustmentMagnitude` parameter\. For example, suppose that you create a policy that adds 25 percent and you specify a minimum amount of 2\. If the scalable target has a capacity of 4 and the scaling policy is performed, 25 percent of 4 is 1\. However, because you specified a minimum increment of 2, Application Auto Scaling adds 2\.

## Cooldown period<a name="step-scaling-cooldown"></a>

The amount of time to wait for a previous scaling activity to take effect is called the cooldown period\. 
+ With scale\-out policies, the intention is to continuously \(but not excessively\) scale out\. After Application Auto Scaling successfully scales out using a step scaling policy, it starts to calculate the cooldown time\. The scaling policy won't increase the desired capacity again unless either a larger scale out is triggered or the cooldown period ends\. While the cooldown period is in effect, capacity added by the initiating scale\-out activity is calculated as part of the desired capacity for the next scale\-out activity\. For example, when an alarm triggers a step scaling policy to increase the capacity by 2, the scaling activity completes successfully, and a cooldown period starts\. If the alarm triggers again during the cooldown period but at a more aggressive step adjustment of 3, the previous increase of 2 is considered part of the current capacity\. Therefore, only 1 is added to the capacity\.
+ With scale\-in policies, the intention is to scale in conservatively to protect your application's availability, so scale\-in activities are blocked until the cooldown period has expired\. However, if another alarm triggers a scale\-out activity during the cooldown period after a scale\-in activity, Application Auto Scaling scales out the target immediately\. In this case, the cooldown period for the scale\-in activity stops and doesn't complete\.

The cooldown period is measured in seconds and applies only to scaling policy\-related scaling activities\. During a cooldown period, when a scheduled action starts at the scheduled time, it can trigger a scaling activity immediately without waiting for the cooldown period to expire\.

## Commonly used commands for scaling policy creation, management, and deletion<a name="step-scaling-policy-commonly-used-commands"></a>

The commonly used commands for working with scaling policies include: 
+ [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to register AWS or custom resources as scalable targets \(a resource that Application Auto Scaling can scale\), and to suspend and resume scaling\. 
+ [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) to add or modify scaling policies for an existing scalable target\.
+  [describe\-scaling\-activities](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) to return information about scaling activities in an AWS Region\. 
+ [describe\-scaling\-policies](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) to return information about scaling policies in an AWS Region\.
+ [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) to delete a scaling\-policy\. 

## Limitations<a name="step-scaling-limitations"></a>

The following are limitations when using step scaling policies:
+ You cannot create step scaling policies for certain services\. Step scaling policies are not supported for DynamoDB, Amazon Comprehend, Lambda, Amazon Keyspaces, Amazon MSK, ElastiCache, or Neptune\.