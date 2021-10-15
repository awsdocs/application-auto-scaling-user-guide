# Getting started with Application Auto Scaling<a name="getting-started"></a>

This topic explains key concepts to help you learn about Application Auto Scaling and start using it\. 

**Scalable target**  <a name="scalabletarget"></a>
An entity that you create to specify the resource that you want to scale\. Each scalable target is uniquely identified by a service namespace, resource ID, and scalable dimension, which represents some capacity dimension of the underlying service\. For example, an Amazon ECS service supports auto scaling of its task count, a DynamoDB table supports auto scaling of the read and write capacity of the table and its global secondary indexes, and an Aurora cluster supports scaling of its replica count\.   
Each scalable target also has a minimum and maximum capacity\. Scaling policies will never go higher or lower than the minimum\-maximum range\. You can make out\-of\-band changes directly to the underlying resource that are outside of this range, which Application Auto Scaling doesn't know about\. However, anytime a scaling policy is invoked or the `RegisterScalableTarget` API is called, Application Auto Scaling retrieves the current capacity and compares it to the minimum and maximum capacity\. If it falls outside of the minimum\-maximum range, then the capacity is updated to comply with the set minimum and maximum\.

**Scale in**  <a name="scalein"></a>
When Application Auto Scaling automatically decreases capacity for a scalable target, the scalable target *scales in*\. The minimum capacity of the scalable target is the lowest capacity a scaling policy can scale in to\.

**Scale out**  <a name="scaleout"></a>
When Application Auto Scaling automatically increases capacity for a scalable target, the scalable target *scales out*\. The maximum capacity of the scalable target is the highest capacity a scaling policy can scale out to\.

**Scaling policy**  <a name="scalingpolicy"></a>
A scaling policy instructs Application Auto Scaling to track a specific CloudWatch metric\. Then, it determines what scaling action to take when the metric is higher or lower than a certain threshold value\. For example, you might want to scale out if the CPU usage across your cluster starts to rise, and scale in when it drops again\.   
The metrics that are used for auto scaling are published by the target service, but you can also publish your own metric to CloudWatch and then use it with a scaling policy\.   
A cooldown period between scaling activities lets the resource stabilize before another scaling activity starts\. Application Auto Scaling continues to evaluate metrics during the cooldown period\. When the cooldown period ends, the scaling policy initiates another scaling activity if needed\. While a cooldown period is in effect, if a larger scale out is necessary based on the current metric value, the scaling policy scales out immediately\.

**Scheduled action**  <a name="scheduledaction"></a>
Scheduled actions automatically scale resources at a specific date and time\. They work by modifying the minimum and maximum capacity for a scalable target, and therefore can be used to scale in and out on a schedule by setting the minimum capacity high or the maximum capacity low\. For example, you can use scheduled actions to scale an application that doesn't consume resources on weekends by decreasing capacity on Friday and increasing capacity on the following Monday\.  
You can also use scheduled actions to optimize the minimum and maximum values over time to adapt to situations where higher than normal traffic is expected, for example, marketing campaigns or seasonal fluctuations\. Doing this can help you improve performance for times when you need to scale out higher for the increasing usage, and reduce costs at times when you use fewer resources\.

## Learn more<a name="getting-started-learn-more"></a>

[AWS services that you can use with Application Auto Scaling](integrated-services-list.md) — This section introduces you to the services that you can scale and helps you set up auto scaling by registering a scalable target\. It also describes each of the IAM service\-linked roles that Application Auto Scaling creates to access resources in the target service\. 

[Target tracking scaling policies for Application Auto Scaling](application-auto-scaling-target-tracking.md) — One of the primary features of Application Auto Scaling is target tracking scaling policies\. Learn how target tracking policies automatically adjust desired capacity to keep utilization at a constant level based on your configured metric and target values\. For example, you can configure target tracking to keep the average CPU utilization for your Spot Fleet at 50 percent\. Application Auto Scaling then launches or terminates EC2 instances as required to keep the aggregated CPU utilization across all servers at 50 percent\.