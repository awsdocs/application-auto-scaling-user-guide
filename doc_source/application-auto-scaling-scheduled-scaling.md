# Scheduled scaling for Application Auto Scaling<a name="application-auto-scaling-scheduled-scaling"></a>

Scaling based on a schedule allows you to set your own scaling schedule according to predictable load changes\. For example, let's say that every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday\. You can configure a schedule for Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday\.

To use scheduled scaling, create *scheduled actions*, which tell Application Auto Scaling to perform scaling activities at specific times\. When you create a scheduled action, you specify the scalable target, when the scaling activity should occur, a minimum capacity, and a maximum capacity\. You can create scheduled actions that scale one time only or that scale on a recurring schedule\. 

At the specified time, Application Auto Scaling scales based on the new capacity values, by comparing current capacity to the specified minimum and maximum capacity\. 
+ If current capacity is less than the specified minimum capacity, Application Auto Scaling scales out \(increases capacity\) to the specified minimum capacity\.
+ If current capacity is greater than the specified maximum capacity, Application Auto Scaling scales in \(decreases capacity\) to the specified maximum capacity\.

You can use scheduled scaling and scaling policies together on the same resource to get the benefits of both\. After a scheduled action runs, the scaling policy can continue to make decisions about whether to further scale capacity\. This helps you ensure that you have sufficient capacity to handle the load for your application\. While your application scales to match demand, current capacity must fall within the minimum and maximum capacity that was set by your scheduled action\. 

For a detailed example of using scheduled scaling, see the blog post [Scheduling AWS Lambda Provisioned Concurrency for recurring peak usage](http://aws.amazon.com/blogs/compute/scheduling-aws-lambda-provisioned-concurrency-for-recurring-peak-usage/) on the AWS Compute Blog\. For a tutorial that walks through how to create scheduled actions using sample AWS resources, see [Getting started using the AWS CLI](get-started-exercise.md)\.

## Considerations<a name="scheduled-scaling-considerations"></a>

When you create a scheduled action, keep the following in mind:
+ A scheduled action sets the `MinCapacity` and `MaxCapacity` to what is specified by the scheduled action at the time specified\. 
+ By default, the times that you set are in Coordinated Universal Time \(UTC\)\. When specifying a one\-time schedule or a recurring schedule with a cron expression, you can change the time zone to correspond to your local time zone or a time zone for another part of your network\. When you specify a time zone that observes daylight saving time, it automatically adjusts for Daylight Saving Time \(DST\)\.
+ If you specify a recurring schedule, you can specify a date and time for the start time, the end time, or both\.
  + The start time and end time must be set in UTC\. Changing the time zone for start and end time is not supported\.
  + If you specify a start time, Application Auto Scaling performs the action at that time, and then performs the action based on the specified recurrence\. 
  + If you specify an end time, the action stops repeating after this time\. Application Auto Scaling does not keep track of previous values and revert back to those previous values after the end time\.
+ The name of the scheduled action must be unique among all other scheduled actions on the specified scalable target\. 
+ Application Auto Scaling guarantees the order in which scheduled actions run for the same scalable target, but cannot guarantee the order in which scheduled actions run across scalable targets\.
+ Due to the distributed nature of Application Auto Scaling and the target services, the delay between the time the scheduled action is triggered and the time the target service honors the scaling action might be a few seconds\. Because scheduled actions are run in the order that they are specified, scheduled actions with start times close to each other can take longer to run\.

## Commonly used commands for scheduled action creation, management, and deletion<a name="scheduled-scaling-commonly-used-commands"></a>

The commonly used commands for working with schedule scaling include: 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to register AWS or custom resources as scalable targets \(a resource that Application Auto Scaling can scale\), and to suspend and resume scaling\. 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) to add or modify scheduled actions for an existing scalable target\.
+  [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) to return information about scaling activities in an AWS Region\. 
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) to return information about scheduled actions in an AWS Region\.
+ [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html) to delete a scheduled action\. 

**Note**  
For brevity, the examples in this guide illustrate CLI commands for a few of the services that integrate with Application Auto Scaling\. To specify a different scalable target, specify its namespace in `--service-namespace`, its scalable dimension in `--scalable-dimension`, and its resource ID in `--resource-id`\. 

## Limitations<a name="scheduled-scaling-limitations"></a>

The following are limitations when using scheduled scaling:
+ Application Auto Scaling doesn't provide second\-level precision in schedule expressions\. The finest resolution using a cron expression is 1 minute\.
+ The scalable target can't be an Amazon MSK cluster\. Scheduled scaling is not supported for Amazon MSK\.