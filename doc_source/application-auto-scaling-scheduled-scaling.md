# Scheduled scaling for Application Auto Scaling<a name="application-auto-scaling-scheduled-scaling"></a>

Scaling based on a schedule allows you to set your own scaling schedule for predictable load changes\. For example, every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday\. You can configure Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday\.

To use scheduled scaling, create *scheduled actions*, which tell Application Auto Scaling to perform scaling activities at specific times\. When you create a scheduled action, you specify the scalable target, when the scaling activity should occur, and the minimum and maximum capacity\. 

At the specified time, Application Auto Scaling scales based on the new capacity values, by comparing current capacity to the specified minimum and maximum capacity\. 
+ If current capacity is less than the specified minimum capacity, Application Auto Scaling scales out \(increases capacity\) to the minimum capacity\.
+ If current capacity is greater than the specified maximum capacity, Application Auto Scaling scales in \(decreases capacity\) to the maximum capacity\.

After the scheduled action runs, the scaling policy \(if any\) can continue to make decisions about whether to further scale capacity\. This helps you ensure that you have sufficient capacity to handle the load for your application\. While your application scales to match demand, current capacity must fall within the minimum and maximum capacity that your scheduled action allows\. 

For a detailed example of using scheduled scaling, see the blog post [Scheduling AWS Lambda Provisioned Concurrency for recurring peak usage](http://aws.amazon.com/blogs/compute/scheduling-aws-lambda-provisioned-concurrency-for-recurring-peak-usage/) on the AWS Compute Blog\.

**Limits**
+ Scheduled scaling is not supported for Amazon MSK\.

## Considerations<a name="sch-actions_rules"></a>

When you create a scheduled action, keep the following in mind:
+ A scheduled action sets the `MinCapacity` and `MaxCapacity` to what is specified by the scheduled action at the time specified by the scheduled action\. It does not keep track of old values and return to the older values after the end time\.
+ A scheduled action may take a few moments to execute\. Because scheduled actions are executed in the order that they are specified, scheduled actions with scheduled start times close to each other can take longer to execute\.
+ Application Auto Scaling guarantees the order of execution for scheduled actions for the same scalable target but not for scheduled actions across scalable targets\.
+ You can temporarily disable scheduled scaling without deleting your scheduled actions\. For more information, see [Suspending and resuming scaling for Application Auto Scaling](application-auto-scaling-suspend-resume-scaling.md)\.
+ For a tutorial that walks through how to create scheduled actions using sample AWS resources, see [Getting started using the AWS CLI](get-started-exercise.md)\.
+ The name of the scheduled action must be unique among all other scheduled actions on the specified scalable target\. 
+ If you specify a recurring schedule and a start time, Application Auto Scaling performs the action at this time, and then performs the action based on the specified recurrence\.

**Note**  
For brevity, the examples in this topic illustrate CLI commands for a few of the services that integrate with Application Auto Scaling\. To specify a different scalable target, specify its namespace in `--service-namespace`, its scalable dimension in `--scalable-dimension`, and its resource ID in `--resource-id`\. 

## Register scalable target<a name="scheduled-scaling-register-scalable-target"></a>

Before you can create a scheduled action, you must register the scalable target\. Use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a new scalable target\. The following example registers the provisioned concurrency for an alias called `BLUE` for the Lambda function called `my-function`\. 

On local Linux, macOS, or Unix machines:

```
aws application-autoscaling register-scalable-target --service-namespace lambda \
  --scalable-dimension lambda:function:ProvisionedConcurrency \
  --resource-id function:my-function:BLUE \
  --min-capacity 10 --max-capacity 50
```

On local Windows machines:

```
aws application-autoscaling register-scalable-target --service-namespace lambda --scalable-dimension lambda:function:ProvisionedConcurrency --resource-id function:my-function:BLUE --min-capacity 10 --max-capacity 50
```

## Using the AWS CLI to create or update a scheduled action<a name="create-scheduled-action"></a>

You can create and update scheduled actions that scale one time only or that scale on a recurring schedule using the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. When you specify the new capacity, you can specify a minimum capacity, a maximum capacity, or both\. 

### Schedule actions for a one\-time schedule<a name="one-time-schedule"></a>

You can specify a one\-time schedule to automatically scale your scalable target at a certain date and time, in UTC\. 

**Example: To scale out one time only**  
The following is an example of creating a scheduled action to scale out capacity at a specific date and time\.   
At the date and time specified for `--schedule`, if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\.  
On local Linux, macOS, or Unix machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource \
  --scalable-dimension custom-resource:ResourceType:Property \
  --resource-id file://~/custom-resource-id.txt \
  --scheduled-action-name scale-out \
  --schedule "at(2021-03-31T22:00:00)" \
  --scalable-target-action MinCapacity=3
```
On local Windows machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource --scalable-dimension custom-resource:ResourceType:Property --resource-id file://~/custom-resource-id.txt --scheduled-action-name scale-out --schedule "at(2021-03-31T22:00:00)" --scalable-target-action MinCapacity=3
```

When you specify a custom resource, the `custom-resource-id.txt` file specifies the API Gateway endpoint for your custom resource\. For more information about configuring a custom resource, see our [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\. The contents of the file may look something like this:

```
https://example.execute-api.region.amazonaws.com/prod/scalableTargetDimensions/1-23456789
```

**Example: To scale in one time only**  
The following is an example of creating a scheduled action to scale in capacity at a specific date and time\.   
At the date and time specified for `--schedule`, if the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.  
On local Linux, macOS, or Unix machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/web-app \
  --scheduled-action-name scale-in \
  --schedule "at(2021-01-31T17:00:00)" \
  --scalable-target-action MaxCapacity=1
```
On local Windows machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app --scheduled-action-name scale-in --schedule "at(2021-01-31T17:00:00)" --scalable-target-action MaxCapacity=1
```

### Recurring schedules using cron or rate expressions<a name="recurrence-schedule"></a>

You can set up a recurring schedule for a scheduled action using schedule expressions\. Application Auto Scaling supports cron and rate formats for schedule expressions\.

**Example: To scale on a recurring schedule using a cron expression**  
You can specify a recurrence schedule, in UTC, using a cron expression\. Call the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command against the scalable target\. Include the `--schedule "cron(cron expression)"` option\. The cron format that's supported by Application Auto Scaling consists of six fields separated by white spaces: \[Minutes\] \[Hours\] \[Day\_of\_Month\] \[Month\] \[Day\_of\_Week\] \[Year\]\. For more information, see [Cron expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) in the *Amazon CloudWatch Events User Guide*\.  
Here are some examples of cron expressions to help\.   


| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
|  0  |  10  |  \*  |  \*  |  ?  |  \*  |  Run at 10:00 am \(UTC\) every day  | 
|  15  |  12  |  \*  |  \*  |  ?  |  \*  |  Run at 12:15 pm \(UTC\) every day  | 
|  0  |  18  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run at 6:00 pm \(UTC\) every Monday through Friday  | 
|  0  |  8  |  1  |  \*  |  ?  |  \*  |  Run at 8:00 am \(UTC\) every 1st day of the month  | 
|  0/15  |  \*  |  \*  |  \*  |  ?  |  \*  |  Run every 15 minutes  | 
|  0/10  |  \*  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 10 minutes Monday through Friday  | 
|  0/5  |  8\-17  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 5 minutes Monday through Friday between 8:00 am and 5:55 pm \(UTC\)  | 
The following is an example of adding a recurring schedule for a scheduled action using a cron expression\.   
On the specified schedule \(every day at 9:00 AM UTC\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.  
On local Linux, macOS, or Unix machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace appstream \
  --scalable-dimension appstream:fleet:DesiredCapacity \
  --resource-id fleet/sample-fleet \
  --scheduled-action-name my-recurring-action \
  --schedule "cron(0 9 * * ? *)" \
  --scalable-target-action MinCapacity=10,MaxCapacity=50
```
On local Windows machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace appstream --scalable-dimension appstream:fleet:DesiredCapacity --resource-id fleet/sample-fleet --scheduled-action-name my-recurring-action --schedule "cron(0 9 * * ? *)" --scalable-target-action MinCapacity=10,MaxCapacity=50
```

**Example: To scale on a recurring schedule using a rate expression**  
Rate expressions schedule scaling at a regular rate\. Call the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command against the scalable target\. Include the `--schedule "rate(value unit)"` option\. The value must be a positive integer\. The unit can be `minute`, `minutes`, `hour`, `hours`, `day`, or `days`\. For more information, see [Rate expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#RateExpressions) in the *Amazon CloudWatch Events User Guide*\.   
The following is an example of adding a recurring schedule for a scheduled action using a rate expression\.   
On the specified schedule \(every 5 hours\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.   
On local Linux, macOS, or Unix machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace dynamodb \
  --scalable-dimension dynamodb:table:WriteCapacityUnits \
  --resource-id table/my-table \
  --scheduled-action-name my-recurring-action \
  --schedule "rate(5 hours)" \
  --start-time 2021-01-30T12:00:00 \
  --end-time 2021-01-31T22:00:00 \
  --scalable-target-action MinCapacity=30,MaxCapacity=100
```
On local Windows machines:  

```
aws application-autoscaling put-scheduled-action --service-namespace dynamodb --scalable-dimension dynamodb:table:WriteCapacityUnits --resource-id table/my-table --scheduled-action-name my-recurring-action --schedule "rate(5 hours)" --start-time 2021-01-30T12:00:00 --end-time 2021-01-31T22:00:00 --scalable-target-action MinCapacity=30,MaxCapacity=100
```

## Describe scheduled actions<a name="describe-scheduled-actions"></a>

You can describe all scheduled actions for the specified service namespace using the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) command\.

```
aws application-autoscaling describe-scheduled-actions --service-namespace ecs
```

The following is example output\.

```
{
    "ScheduledActions": [
        {
            "ScheduledActionARN": "<arn>",
            "ServiceNamespace": "ecs",
            "CreationTime": 1596319455.951,
            "ScalableDimension": "ecs:service:DesiredCount",
            "Schedule": "at(2021-01-31T17:00:00)",
            "ScalableTargetAction": {
                "MaxCapacity": 1
            },
            "ScheduledActionName": "scale-in",
            "ResourceId": "service/default/web-app"
        }
    ]
}
```

## Delete a scheduled action<a name="delete-scheduled-action"></a>

When you are finished with a scheduled action, you can delete it using the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\.

On local Linux, macOS, or Unix machines:

```
aws application-autoscaling delete-scheduled-action --service-namespace ec2 \
  --scalable-dimension ec2:spot-fleet-request:TargetCapacity \
  --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
  --scheduled-action-name my-spot-fleet-action
```

On local Windows machines:

```
aws application-autoscaling delete-scheduled-action --service-namespace ec2 --scalable-dimension ec2:spot-fleet-request:TargetCapacity --resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE --scheduled-action-name my-spot-fleet-action
```