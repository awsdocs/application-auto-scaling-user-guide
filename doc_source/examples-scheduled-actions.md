# Example scheduled actions for Application Auto Scaling<a name="examples-scheduled-actions"></a>

The following examples show how to create scheduled actions with the AWS CLI [put\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. When you specify the new capacity, you can specify a minimum capacity, a maximum capacity, or both\.

**Topics**
+ [Creating a scheduled action that occurs only once](#one-time-schedule)
+ [Creating a scheduled action that runs on a recurring interval](#recurrence-schedule-rate)
+ [Creating a scheduled action that runs on a recurring schedule](#recurrence-schedule-cron)
+ [Creating a one\-time scheduled action that specifies a time zone](#one-time-schedule-set-time-zone)
+ [Creating a recurring scheduled action that specifies a time zone](#recurring-schedule-set-time-zone)

## Creating a scheduled action that occurs only once<a name="one-time-schedule"></a>

To automatically scale your scalable target one time only, at a specified date and time, use the `--schedule "at(yyyy-mm-ddThh:mm:ss)"` option\.

**Example: To scale out one time only**  
The following is an example of creating a scheduled action to scale out capacity at a specific date and time\.   
At the date and time specified for `--schedule` \(10:00 PM UTC on March 31, 2021\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\.  
**Linux, macOS, or Unix**  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource \
  --scalable-dimension custom-resource:ResourceType:Property \
  --resource-id file://~/custom-resource-id.txt \
  --scheduled-action-name scale-out \
  --schedule "at(2021-03-31T22:00:00)" \
  --scalable-target-action MinCapacity=3
```
**Windows**  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource --scalable-dimension custom-resource:ResourceType:Property --resource-id file://~/custom-resource-id.txt --scheduled-action-name scale-out --schedule "at(2021-03-31T22:00:00)" --scalable-target-action MinCapacity=3
```
When this scheduled action runs, if the maximum capacity is less than the value specified for minimum capacity, you must specify a new minimum and maximum capacity, and not just the minimum capacity\. 

**Example: To scale in one time only**  
The following is an example of creating a scheduled action to scale in capacity at a specific date and time\.   
At the date and time specified for `--schedule` \(10:30 PM UTC on March 31, 2021\), if the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.  
**Linux, macOS, or Unix**  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource \
  --scalable-dimension custom-resource:ResourceType:Property \
  --resource-id file://~/custom-resource-id.txt \
  --scheduled-action-name scale-in \
  --schedule "at(2021-03-31T22:30:00)" \
  --scalable-target-action MinCapacity=0,MaxCapacity=0
```
**Windows**  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource --scalable-dimension custom-resource:ResourceType:Property --resource-id file://~/custom-resource-id.txt --scheduled-action-name scale-in --schedule "at(2021-03-31T22:30:00)" --scalable-target-action MinCapacity=0,MaxCapacity=0
```

## Creating a scheduled action that runs on a recurring interval<a name="recurrence-schedule-rate"></a>

To schedule scaling at a recurring interval, use the `--schedule "rate(value unit)"` option\. The value must be a positive integer\. The unit can be `minute`, `minutes`, `hour`, `hours`, `day`, or `days`\. For more information, see [Rate expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#RateExpressions) in the *Amazon CloudWatch Events User Guide*\. 

The following is an example of a scheduled action that uses a rate expression\. 

On the specified schedule \(every 5 hours starting on January 30, 2021 at 12:00 PM UTC and ending on January 31, 2021 at 10:00 PM UTC\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\. 

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scheduled-action --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/web-app \
  --scheduled-action-name my-recurring-action \
  --schedule "rate(5 hours)" \
  --start-time 2021-01-30T12:00:00 \
  --end-time 2021-01-31T22:00:00 \
  --scalable-target-action MinCapacity=3,MaxCapacity=10
```

**Windows**

```
aws application-autoscaling put-scheduled-action --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app --scheduled-action-name my-recurring-action --schedule "rate(5 hours)" --start-time 2021-01-30T12:00:00 --end-time 2021-01-31T22:00:00 --scalable-target-action MinCapacity=3,MaxCapacity=10
```

## Creating a scheduled action that runs on a recurring schedule<a name="recurrence-schedule-cron"></a>

To schedule scaling on a recurring schedule, use the `--schedule "cron(fields)"` option\. The cron format that's supported by Application Auto Scaling consists of six fields separated by white spaces: \[Minutes\] \[Hours\] \[Day\_of\_Month\] \[Month\] \[Day\_of\_Week\] \[Year\]\.

Here are some examples of cron expressions\. 


| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
|  0  |  10  |  \*  |  \*  |  ?  |  \*  |  Run at 10:00 am \(UTC\) every day  | 
|  15  |  12  |  \*  |  \*  |  ?  |  \*  |  Run at 12:15 pm \(UTC\) every day  | 
|  0  |  18  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run at 6:00 pm \(UTC\) every Monday through Friday  | 
|  0  |  8  |  1  |  \*  |  ?  |  \*  |  Run at 8:00 am \(UTC\) the 1st day of every month  | 
|  0/15  |  \*  |  \*  |  \*  |  ?  |  \*  |  Run every 15 minutes  | 
|  0/10  |  \*  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 10 minutes Monday through Friday  | 
|  0/5  |  8\-17  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 5 minutes Monday through Friday between 8:00 am and 5:55 pm \(UTC\)   | 

For more information about writing cron expressions, see [Cron expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) in the *Amazon CloudWatch Events User Guide*\.

The following is an example of a scheduled action that uses a cron expression\. 

On the specified schedule \(every day at 9:00 AM UTC\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scheduled-action --service-namespace appstream \
  --scalable-dimension appstream:fleet:DesiredCapacity \
  --resource-id fleet/sample-fleet \
  --scheduled-action-name my-recurring-action \
  --schedule "cron(0 9 * * ? *)" \
  --scalable-target-action MinCapacity=10,MaxCapacity=50
```

**Windows**

```
aws application-autoscaling put-scheduled-action --service-namespace appstream --scalable-dimension appstream:fleet:DesiredCapacity --resource-id fleet/sample-fleet --scheduled-action-name my-recurring-action --schedule "cron(0 9 * * ? *)" --scalable-target-action MinCapacity=10,MaxCapacity=50
```

## Creating a one\-time scheduled action that specifies a time zone<a name="one-time-schedule-set-time-zone"></a>

Scheduled actions are set to the UTC time zone by default\. To specify a different time zone, include the `--timezone` option and specify the canonical name for the time zone \(`America/New_York`, for example\)\. For more information, see [https://www\.joda\.org/joda\-time/timezones\.html](https://www.joda.org/joda-time/timezones.html), which provides information about the IANA time zones that are supported when calling [put\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html)\.

The following is an example that uses the `--timezone` option when creating a scheduled action to scale capacity at a specific date and time\. 

At the date and time specified for `--schedule` \(5:00 PM local time on January 31, 2021\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scheduled-action --service-namespace comprehend \
  --scalable-dimension comprehend:document-classifier-endpoint:DesiredInferenceUnits \
  --resource-id arn:aws:comprehend:us-west-2:123456789012:document-classifier-endpoint/EXAMPLE \
  --scheduled-action-name  my-one-time-action \
  --schedule "at(2021-01-31T17:00:00)" --timezone "America/New_York" \
  --scalable-target-action MinCapacity=1,MaxCapacity=3
```

**Windows**

```
aws application-autoscaling put-scheduled-action --service-namespace comprehend --scalable-dimension comprehend:document-classifier-endpoint:DesiredInferenceUnits --resource-id arn:aws:comprehend:us-west-2:123456789012:document-classifier-endpoint/EXAMPLE --scheduled-action-name  my-one-time-action --schedule "at(2021-01-31T17:00:00)" --timezone "America/New_York" --scalable-target-action MinCapacity=1,MaxCapacity=3
```

## Creating a recurring scheduled action that specifies a time zone<a name="recurring-schedule-set-time-zone"></a>

The following is an example that uses the `--timezone` option when creating a recurring scheduled action to scale capacity\.

On the specified schedule \(every Monday through Friday at 6:00 PM local time\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.

**Linux, macOS, or Unix**

```
aws application-autoscaling put-scheduled-action --service-namespace lambda \
  --scalable-dimension lambda:function:ProvisionedConcurrency \
  --resource-id function:my-function:BLUE \
  --scheduled-action-name my-recurring-action \
  --schedule "cron(0 18 ? * MON-FRI *)" --timezone "Etc/GMT+9" \
  --scalable-target-action MinCapacity=10,MaxCapacity=50
```

**Windows**

```
aws application-autoscaling put-scheduled-action --service-namespace lambda --scalable-dimension lambda:function:ProvisionedConcurrency --resource-id function:my-function:BLUE --scheduled-action-name my-recurring-action --schedule "cron(0 18 ? * MON-FRI *)" --timezone "Etc/GMT+9" --scalable-target-action MinCapacity=10,MaxCapacity=50
```
