# Scheduled scaling for Application Auto Scaling<a name="application-auto-scaling-scheduled-scaling"></a>

Scaling based on a schedule allows you to set your own scaling schedule according to predictable load changes\. For example, let's say that every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday\. You can configure a schedule for Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday\.

To use scheduled scaling, create *scheduled actions*, which tell Application Auto Scaling to perform scaling activities at specific times\. When you create a scheduled action, you specify the scalable target, when the scaling activity should occur, a minimum capacity, and a maximum capacity\. You can create scheduled actions that scale one time only or that scale on a recurring schedule\. 

At the specified time, Application Auto Scaling scales based on the new capacity values, by comparing current capacity to the specified minimum and maximum capacity\. 
+ If current capacity is less than the specified minimum capacity, Application Auto Scaling scales out \(increases capacity\) to the specified minimum capacity\.
+ If current capacity is greater than the specified maximum capacity, Application Auto Scaling scales in \(decreases capacity\) to the specified maximum capacity\.

You can use scheduled scaling and scaling policies together on the same resource to get the benefits of both\. After a scheduled action runs, the scaling policy can continue to make decisions about whether to further scale capacity\. This helps you ensure that you have sufficient capacity to handle the load for your application\. While your application scales to match demand, current capacity must fall within the minimum and maximum capacity that was set by your scheduled action\. 

For a detailed example of using scheduled scaling, see the blog post [Scheduling AWS Lambda Provisioned Concurrency for recurring peak usage](http://aws.amazon.com/blogs/compute/scheduling-aws-lambda-provisioned-concurrency-for-recurring-peak-usage/) on the AWS Compute Blog\. For a tutorial that walks through how to create scheduled actions using sample AWS resources, see [Tutorial: Getting started with scheduled scaling using the AWS CLI](get-started-exercise.md)\.

## Considerations<a name="scheduled-scaling-considerations"></a>

When you create a scheduled action, keep the following in mind:
+ A scheduled action sets the `MinCapacity` and `MaxCapacity` to what is specified by the scheduled action at the date and time specified\. The request can optionally include only one of these sizes\. For example, you can create a scheduled action with only the minimum capacity specified\. In some cases, however, you must include both sizes to ensure that the new minimum capacity is not greater than the maximum capacity, or the new maximum capacity is not less than the minimum capacity\.
+ By default, the recurring schedules that you set are in Coordinated Universal Time \(UTC\)\. You can change the time zone to correspond to your local time zone or a time zone for another part of your network\. When you specify a time zone that observes daylight saving time, the action automatically adjusts for Daylight Saving Time \(DST\)\. For more information, see [Schedule recurring scaling actions using cron expressions](scheduled-scaling-using-cron-expressions.md)\.
+ You can temporarily turn off scheduled scaling for a scalable target\. This helps you prevent scheduled actions from being active without having to delete them\. You can then resume scheduled scaling when you want to use it again\. For more information, see [Suspending and resuming scaling for Application Auto Scaling](application-auto-scaling-suspend-resume-scaling.md)\.
+ The order in which scheduled actions run is guaranteed for the same scalable target, but not for scheduled actions across scalable targets\.
+ To complete a scheduled action successfully, the specified resource must be in a scalable state in the target service\. If it isn't, the request fails and returns an error message, for example, `Resource Id [ActualResourceId] is not scalable. Reason: The status of all DB instances must be 'available' or 'incompatible-parameters'`\.
+ Due to the distributed nature of Application Auto Scaling and the target services, the delay between the time the scheduled action is triggered and the time the target service honors the scaling action might be a few seconds\. Because scheduled actions are run in the order that they are specified, scheduled actions with start times close to each other can take longer to run\.

## Commonly used commands for scheduled action creation, management, and deletion<a name="scheduled-scaling-commonly-used-commands"></a>

The commonly used commands for working with schedule scaling include: 
+ [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to register AWS or custom resources as scalable targets \(a resource that Application Auto Scaling can scale\), and to suspend and resume scaling\. 
+ [put\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) to add or modify scheduled actions for an existing scalable target\.
+  [describe\-scaling\-activities](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) to return information about scaling activities in an AWS Region\. 
+ [describe\-scheduled\-actions](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) to return information about scheduled actions in an AWS Region\.
+ [delete\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html) to delete a scheduled action\. 

## Limitations<a name="scheduled-scaling-limitations"></a>

The following are limitations when using scheduled scaling:
+ The names of scheduled actions must be unique per scalable target\.
+ Application Auto Scaling doesn't provide second\-level precision in schedule expressions\. The finest resolution using a cron expression is 1 minute\.
+ The scalable target can't be an Amazon MSK cluster\. Scheduled scaling is not supported for Amazon MSK\.