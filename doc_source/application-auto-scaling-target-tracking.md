# Target tracking scaling policies for Application Auto Scaling<a name="application-auto-scaling-target-tracking"></a>

To create a target tracking scaling policy, you specify an Amazon CloudWatch metric and a target value that represents the ideal average utilization or throughput level for your application\. Application Auto Scaling can then scale out the scalable target \(add capacity\) to handle peak traffic, and scale in the scalable target \(remove capacity\) to reduce costs during periods of low utilization or throughput\.

For example, let's say that you currently have an application that runs on Spot Fleet, and you want the CPU utilization of the fleet to stay at around 50 percent when the load on the application changes\. This gives you extra capacity to handle traffic spikes without maintaining an excessive number of idle resources\. 

You can meet this need by creating a target tracking scaling policy that targets an average CPU utilization of 50 percent\. Then, Application Auto Scaling scales the number of instances to keep the actual metric value at or near 50 percent\. 

The following diagram shows an overview of how a target tracking scaling policy works when the set up is complete\.

![\[Overview diagram of a target tracking scaling policy.\]](http://docs.aws.amazon.com/autoscaling/application/userguide/images/target_tracking_scaling_policy.png)

## Considerations<a name="target-tracking-considerations"></a>

The following considerations apply when working with target tracking scaling policies:
+ Do not create, edit, or delete the CloudWatch alarms that are used with a target tracking scaling policy\. Application Auto Scaling creates and manages the CloudWatch alarms that are associated with your target tracking scaling policies and deletes them when no longer needed\.
+ To ensure application availability, Application Auto Scaling scales out proportionally to the metric as fast as it can, but scales in more gradually\.
+ If the metric is missing data points, this causes the CloudWatch alarm state to change to `INSUFFICIENT_DATA`\. When this happens, Application Auto Scaling cannot scale your scalable target until new data points are found\. For information about creating alarms when there is insufficient data, see [Monitoring with CloudWatch alarms](cloudwatch-alarm-notifications.md)\.
+ You may see gaps between the target value and the actual metric data points\. This is because Application Auto Scaling always acts conservatively by rounding up or down when it determines how much capacity to add or remove\. This prevents it from adding insufficient capacity or removing too much capacity\. However, for a scalable target with a small capacity, the actual metric data points might seem far from the target value\. 
+ For a scalable target with a larger capacity, adding or removing capacity causes less of a gap between the target value and the actual metric data points\.
+ A target tracking scaling policy assumes that it should perform scale out when the specified metric is above the target value\. You cannot use a target tracking scaling policy to scale out when the specified metric is below the target value\.

**Multiple scaling policies**
+ You can have multiple target tracking scaling policies for a scalable target, provided that each of them uses a different metric\. The intention of Application Auto Scaling is to always prioritize availability, so its behavior differs depending on whether the target tracking policies are ready for scale out or scale in\. It will scale out the scalable target if any of the target tracking policies are ready for scale out, but will scale in only if all of the target tracking policies \(with the scale\-in portion enabled\) are ready to scale in\. 
+ If multiple scaling policies instruct the scalable target to scale out or in at the same time, Application Auto Scaling scales based on the policy that provides the largest capacity for both scale in and scale out\. This provides greater flexibility to cover multiple scenarios and ensures that there is always enough capacity to process your workloads\. 
+ You can disable the scale\-in portion of a target tracking scaling policy\. This feature provides the flexibility to use a different method for scale in than you use for scale out\. For example, you can use a step scaling policy for scale in while using a target tracking scaling policy for scale out\. 
+ We recommend caution, however, when using target tracking scaling policies with step scaling policies because conflicts between these policies can cause undesirable behavior\. For example, if the step scaling policy initiates a scale\-in activity before the target tracking policy is ready to scale in, the scale\-in activity will not be blocked\. After the scale\-in activity completes, the target tracking policy could instruct the scalable target to scale out again\. 

**Supporting application availability during high utilization periods**

A target tracking scaling policy is more aggressive in adding capacity when utilization increases than it is in removing capacity when utilization decreases\. For example, if the policy's specified metric reaches its target value, the policy assumes that your application is already heavily loaded\. So it responds by adding capacity proportional to the metric value as fast as it can\. The higher the metric, the more capacity is added\.

When the metric falls below the target value, the policy expects that utilization will eventually increase again\. In this case, it slows down scaling by removing capacity only when utilization passes a threshold that is far enough below the target value \(usually more than 10% lower\) for utilization to be considered to have slowed\. The intention of this more conservative behavior is to ensure that removing capacity only happens when the application is no longer experiencing demand at the same high level that it was previously\.

For workloads that are cyclical in nature, you also have the option to automate capacity changes on a schedule using scheduled scaling\. For each scheduled action, a new minimum capacity value and a new maximum capacity value can be defined\. These values form the boundaries of the scaling policy\. 

The combination of scheduled scaling and target tracking scaling can help reduce the impact of a sharp increase in utilization levels, when capacity is needed immediately\.

## Choose metrics<a name="target-tracking-choose-metrics"></a>

You can create target tracking scaling policies with either predefined metrics or custom metrics\. 

When you create a target tracking scaling policy with a predefined metric, you choose one metric from the following list of predefined metrics\. 

AppStream 2\.0  
`AppStreamAverageCapacityUtilization` \(Percent\)

Aurora  
`RDSReaderAverageCPUUtilization` \(Percent\)  
`RDSReaderAverageDatabaseConnections` \(Count\)

Amazon Comprehend  
`ComprehendInferenceUtilization` \(Percent\)

DynamoDB  
`DynamoDBReadCapacityUtilization` \(Percent\)  
`DynamoDBWriteCapacityUtilization` \(Percent\)

Amazon ECS  
`ALBRequestCountPerTarget` \(Count\)  
`ECSServiceAverageCPUUtilization` \(Percent\)  
`ECSServiceAverageMemoryUtilization` \(Percent\)

ElastiCache  
`ElastiCachePrimaryEngineCPUUtilization` \(Percent\)  
`ElastiCacheReplicaEngineCPUUtilization` \(Percent\)  
`ElastiCacheDatabaseMemoryUsageCountedForEvictPercentage` \(Percent\)

Amazon Keyspaces \(for Apache Cassandra\)  
`CassandraReadCapacityUtilization` \(Percent\)  
`CassandraWriteCapacityUtilization` \(Percent\)

Lambda  
`LambdaProvisionedConcurrencyUtilization` \(Percent\)

Amazon Managed Streaming for Apache Kafka \(MSK\)  
`KafkaBrokerStorageUtilization` \(Percent\)

Neptune  
`NeptuneReaderAverageCPUUtilization` \(Percent\)

Spot Fleet \(Amazon EC2\)  
`ALBRequestCountPerTarget` \(Count\)  
`EC2SpotFleetRequestAverageCPUUtilization` \(Percent\)  
`EC2SpotFleetRequestAverageNetworkIn` \(Count\)  
`EC2SpotFleetRequestAverageNetworkOut` \(Count\)

SageMaker  
`SageMakerVariantInvocationsPerInstance` \(Count\)

**Tip**  
Each predefined metric is based on a metric published by the respective service\. For more information about these metrics, see the documentation for the service you are using available from the table in [Metrics and dimensions](monitoring-cloudwatch.md#metrics-to-monitor)\.

Keep the following in mind when choosing a metric:
+ Not all metrics work for target tracking\. This can be important when you are specifying a custom metric\. The metric must be a valid utilization metric and describe how busy a scalable target is\. The metric value must increase or decrease proportionally to the capacity of the scalable target so that the metric data can be used to proportionally scale the scalable target\. 
+ To use the `ALBRequestCountPerTarget` metric, you must specify the `ResourceLabel` parameter to identify the target group that is associated with the metric\. 
+ When a metric emits real 0 values to CloudWatch \(for example, `ALBRequestCountPerTarget`\), Application Auto Scaling can scale in to 0 when there is no traffic to your application\. To have your scalable target scale in to 0 when no requests are routed it, the scalable target's minimum capacity must be set to 0\. 
+ When you create a target tracking scaling policy with a custom metric, you can use metric math to combine metrics\. For more information, see [Create a target tracking scaling policy for Application Auto Scaling using metric math](application-auto-scaling-target-tracking-metric-math.md)\.
+ To see whether the service you are using supports specifying a custom metric in the console, consult the documentation for that service\. 

**Use detailed monitoring for Amazon EC2**

When you use EC2 instance metrics in your scaling policies, we recommend that you configure these metrics with a 1\-minute granularity to ensure a faster response to changes in the metric value\. Scaling on instance metrics with a 5\-minute granularity can result in slower response times and scaling on stale metric data\.

To get this level of data for Amazon EC2 metrics, you must specifically enable detailed monitoring\. By default, EC2 instances are enabled for basic monitoring, which means metric data for instances is available at 5\-minute granularity\. For more information, see [Enable or turn off detailed monitoring for your instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Define target value<a name="target-tracking-define-target-value"></a>

When you create a target tracking scaling policy, you must specify a target value\. The target value represents the optimal average utilization or throughput for your application\. To use resources cost efficiently, set the target value as high as possible with a reasonable buffer for unexpected traffic increases\. When your application is optimally scaled out for a normal traffic flow, the actual metric value should be at or just below the target value\.

When a scaling policy is based on throughput, such as the request count per target for an Application Load Balancer, network I/O, or other count metrics, the target value represents the optimal average throughput from a single entity \(such as a single target of your Application Load Balancer target group\), for a one\-minute period\.

## Define cooldown period<a name="target-tracking-cooldown"></a>

The amount of time to wait for a previous scaling activity to take effect is called the cooldown period\. 

With target tracking scaling policies, there are two types of cooldown periods:
+ With the *scale\-out cooldown period*, the intention is to continuously \(but not excessively\) scale out\. After Application Auto Scaling successfully scales out using a target tracking scaling policy, it starts to calculate the cooldown time\. The scaling policy won't increase the desired capacity again unless either a larger scale out is triggered or the cooldown period ends\. While the scale\-out cooldown period is in effect, the capacity added by the initiating scale\-out activity is calculated as part of the desired capacity for the next scale\-out activity\.
+ With the *scale\-in cooldown period*, the intention is to scale in conservatively to protect your application's availability, so scale\-in activities are blocked until the cooldown period has expired\. However, if another alarm triggers a scale\-out activity during the scale\-in cooldown period, Application Auto Scaling scales out the target immediately\. In this case, the scale\-in cooldown period stops and doesn't complete\.

Each cooldown period is measured in seconds and applies only to scaling policy\-related scaling activities\. During a cooldown period, when a scheduled action starts at the scheduled time, it can trigger a scaling activity immediately without waiting for the cooldown period to expire\.

You can start with the default values, which can be later fine\-tuned\. For example, you might need to increase a cooldown period to prevent your target tracking scaling policy from being too aggressive about changes that occur over short periods of time\. For the default values, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\.

## Commonly used commands for scaling policy creation, management, and deletion<a name="target-tracking-policy-commonly-used-commands"></a>

The commonly used commands for working with scaling policies include: 
+ [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to register AWS or custom resources as scalable targets \(a resource that Application Auto Scaling can scale\), and to suspend and resume scaling\. 
+ [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) to add or modify scaling policies for an existing scalable target\.
+  [describe\-scaling\-activities](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) to return information about scaling activities in an AWS Region\. 
+ [describe\-scaling\-policies](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) to return information about scaling policies in an AWS Region\.
+ [delete\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) to delete a scaling\-policy\. 

## Limitations<a name="target-tracking-limitations"></a>

The following are limitations when using target tracking scaling policies:
+ The scalable target can't be an Amazon EMR cluster\. Target tracking scaling policies are not supported for Amazon EMR\.
+ When an Amazon MSK cluster is the scalable target, scale in is disabled and cannot be enabled\.
+ You cannot use the `RegisterScalableTarget` or `PutScalingPolicy` API operations to update an AWS Auto Scaling scaling plan\. For information about using scaling plans, see the [AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/index.html#lang/en_us) documentation\.