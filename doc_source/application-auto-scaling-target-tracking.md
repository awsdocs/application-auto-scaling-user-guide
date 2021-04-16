# Target tracking scaling policies for Application Auto Scaling<a name="application-auto-scaling-target-tracking"></a>

With target tracking scaling policies, you choose a scaling metric and set a target value\. Application Auto Scaling creates and manages the CloudWatch alarms that trigger the scaling policy and calculates the scaling adjustment based on the metric and the target value\. The scaling policy adds or removes capacity as required to keep the metric at, or close to, the specified target value\. In addition to keeping the metric close to the target value, a target tracking scaling policy also adjusts to changes in the metric due to a changing load pattern\.

## Choosing metrics<a name="available-metrics"></a>

The following predefined metrics are available for use when you create target tracking scaling policies\. You can optionally define which metric to monitor and use with your target tracking scaling policy by using a custom metric specification\. 

**Predefined metric names**
+ `DynamoDBReadCapacityUtilization`
+ `DynamoDBWriteCapacityUtilization`
+ `ALBRequestCountPerTarget`
+ `RDSReaderAverageCPUUtilization`
+ `RDSReaderAverageDatabaseConnections`
+ `EC2SpotFleetRequestAverageCPUUtilization`
+ `EC2SpotFleetRequestAverageNetworkIn`
+ `EC2SpotFleetRequestAverageNetworkOut`
+ `SageMakerVariantInvocationsPerInstance`
+ `ECSServiceAverageCPUUtilization`
+ `ECSServiceAverageMemoryUtilization`
+ `AppStreamAverageCapacityUtilization`
+ `ComprehendInferenceUtilization`
+ `LambdaProvisionedConcurrencyUtilization`
+ `CassandraReadCapacityUtilization`
+ `CassandraWriteCapacityUtilization`
+ `KafkaBrokerStorageUtilization`

Each metric represents a time\-ordered set of data points stored in Amazon CloudWatch\. While most of the metrics in AWS are reported every minute by default, Amazon EC2 metrics are reported every five minutes by default\. For an additional charge, you can enable detailed monitoring to get metric data for instances at a one minute frequency\. To ensure a faster response to utilization changes, we recommend that you enable detailed monitoring\. For more information, see [Enable or turn off detailed monitoring for your instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html) in the *Amazon EC2 User Guide for Linux Instances*\.

Keep the following in mind when choosing a metric:
+ Not all metrics work for target tracking\. This can be important when you are specifying a custom metric\. The metric must be a valid utilization metric and describe how busy a scalable target is\. The metric value must increase or decrease proportionally to the capacity of the scalable target so that the metric data can be used to proportionally scale the scalable target\. 
+ To use the `ALBRequestCountPerTarget` metric, you must specify the `ResourceLabel` parameter to identify the target group that is associated with the metric\. 
+ When a metric emits real 0 values to CloudWatch \(for example, `ALBRequestCountPerTarget`\), Application Auto Scaling can scale in to 0 when there is no traffic to your application\. To have your scalable target scale in to 0 when no requests are routed it, the scalable target's minimum capacity must be set to 0\. 
+ Not all services allow you to manage custom metrics through the console of the target service\. To see if the target service supports custom metrics in the console, consult the documentation for that service\.

## Considerations<a name="target-tracking-considerations"></a>

Keep the following considerations in mind:
+ A target tracking scaling policy assumes that it should perform scale out when the specified metric is above the target value\. You cannot use a target tracking scaling policy to scale out when the specified metric is below the target value\.
+ You may see gaps between the target value and the actual metric data points\. This is because Application Auto Scaling always acts conservatively by rounding up or down when it determines how much capacity to add or remove\. This prevents it from adding insufficient capacity or removing too much capacity\. However, for a scalable target with a small capacity, the actual metric data points might seem far from the target value\. 
+ For a scalable target with a larger capacity, adding or removing capacity causes less of a gap between the target value and the actual metric data points\.
+ To ensure application availability, Application Auto Scaling scales out proportionally to the metric as fast as it can, but scales in more gradually\.
+ You can have multiple target tracking scaling policies for a scalable target, provided that each of them uses a different metric\. The intention of Application Auto Scaling is to always prioritize availability, so its behavior differs depending on whether the target tracking policies are ready for scale out or scale in\. It will scale out the scalable target if any of the target tracking policies are ready for scale out, but will scale in only if all of the target tracking policies \(with the scale\-in portion enabled\) are ready to scale in\. 
+ If multiple policies instruct the scalable target to scale out or in at the same time, Application Auto Scaling scales based on the policy that provides the largest capacity for both scale in and scale out\. This provides greater flexibility to cover multiple scenarios and ensures that there is always enough capacity to process your application workloads\. 
+ You can disable the scale\-in portion of a target tracking scaling policy\. This feature provides the flexibility to use a different method for scale in than you use for scale out\. For example, you can use a step scaling policy for scale in while using a target tracking scaling policy for scale out\. 
+ We recommend caution, however, when using target tracking scaling policies with step scaling policies because conflicts between these policies can cause undesirable behavior\. For example, if the step scaling policy initiates a scale\-in activity before the target tracking policy is ready to scale in, the scale\-in activity will not be blocked\. After the scale\-in activity completes, the target tracking policy could instruct the scalable target to scale out again\. 
+ A target tracking scaling policy does not scale in when the state of the alarm is `INSUFFICIENT_DATA`\. For more information, see [Monitoring with CloudWatch alarms](cloudwatch-alarm-notifications.md)\.
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

When the metric falls below the target value, the policy expects that utilization will eventually increase again\. So it slows down scaling by removing capacity only when utilization passes a threshold that is far enough below the target value \(usually more than 10% lower\) for utilization to be considered to have slowed\. The intention of this more conservative behavior is to ensure that removing capacity only happens when the application is no longer experiencing demand at the same high level that it was previously\. This is currently the default behavior for all target tracking scaling policies \(though the behavior could change in the future\)\.

For workloads that are cyclical in nature, you also have the option to automate capacity changes on a schedule using scheduled scaling\. For each scheduled action, a new minimum capacity value and a new maximum capacity value can be defined\. These values form the boundaries of the scaling policy\. 

The combination of scheduled scaling and target tracking scaling can help reduce the impact of a sharp increase in utilization levels, when capacity is needed immediately\.

## Commonly used commands for scaling policy creation, management, and deletion<a name="target-tracking-policy-commonly-used-commands"></a>

The commonly used commands for working with scaling policies include: 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to register AWS or custom resources as scalable targets \(a resource that Application Auto Scaling can scale\), and to suspend and resume scaling\. 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) to add or modify scaling policies for an existing scalable target\.
+  [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) to return information about scaling activities in an AWS Region\. 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) to return information about scaling policies in an AWS Region\.
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) to delete a scaling\-policy\. 

## Limitations<a name="target-tracking-limitations"></a>

The following are limitations when using target tracking scaling policies:
+ The scalable target can't be an Amazon EMR cluster\. Target tracking scaling policies are not supported for Amazon EMR\.
+ When an Amazon MSK cluster is the scalable target, scale in is disabled and cannot be enabled\.
+ You cannot use the `RegisterScalableTarget` or `PutScalingPolicy` API operations to update an AWS Auto Scaling scaling plan\. For information about using scaling plans, see the [AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/index.html#lang/en_us) documentation\.