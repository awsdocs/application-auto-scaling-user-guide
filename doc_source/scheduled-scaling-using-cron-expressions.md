# Schedule recurring scaling actions using cron expressions<a name="scheduled-scaling-using-cron-expressions"></a>

You can create scheduled actions that run on a recurring schedule using a cron expression\. 

To create a recurring schedule, specify a cron expression and a time zone to describe when that scheduled action is to recur\. The supported time zone values are the canonical names of the IANA time zones supported by [Joda\-Time](https://www.joda.org/joda-time/timezones.html) \(such as `Etc/GMT+9` or `Pacific/Tahiti`\)\. You can optionally specify a date and time for the start time, the end time, or both\. For an example command that uses the AWS CLI to create a scheduled action, see [Create a recurring scheduled action that specifies a time zone](examples-scheduled-actions.md#recurring-schedule-set-time-zone)\.

The supported cron expression format consists of six fields separated by white spaces: \[Minutes\] \[Hours\] \[Day\_of\_Month\] \[Month\] \[Day\_of\_Week\] \[Year\]\. For example, the cron expression `30 6 * * 2 *` configures a scheduled action that recurs every Tuesday at 6:30 AM\. The asterisk is used as a wildcard to match all values for a field\. For more information about writing cron expressions, see [Cron expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) in the *Amazon CloudWatch Events User Guide*\.

When you create a recurring schedule, choose your start and end times carefully\. Keep the following in mind:
+ If you specify a start time, Application Auto Scaling performs the action at this time, and then performs the action based on the specified recurrence\. 
+ If you specify an end time, the action stops repeating after this time\. Application Auto Scaling does not keep track of previous values and revert back to those previous values after the end time\. 
+ The start time and end time must be set in UTC when you use the AWS CLI or the AWS SDKs to create or update a scheduled action\. 

**Examples**  
You can refer to the following table when you create a recurring schedule for an Application Auto Scaling scalable target\. The following examples are the correct syntax for using Application Auto Scaling to create or update a scheduled action\.


| Minutes | Hours | Day of month | Month | Day of week | Year | Meaning | 
| --- | --- | --- | --- | --- | --- | --- | 
|  0  |  10  |  \*  |  \*  |  ?  |  \*  |  Run at 10:00 am \(UTC\) every day  | 
|  15  |  12  |  \*  |  \*  |  ?  |  \*  |  Run at 12:15 pm \(UTC\) every day  | 
|  0  |  18  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run at 6:00 pm \(UTC\) every Monday through Friday  | 
|  0  |  8  |  1  |  \*  |  ?  |  \*  |  Run at 8:00 am \(UTC\) the 1st day of every month  | 
|  0/15  |  \*  |  \*  |  \*  |  ?  |  \*  |  Run every 15 minutes  | 
|  0/10  |  \*  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 10 minutes Monday through Friday  | 
|  0/5  |  8\-17  |  ?  |  \*  |  MON\-FRI  |  \*  |  Run every 5 minutes Monday through Friday between 8:00 am and 5:55 pm \(UTC\)   | 

**Exception**  
You can also create a cron expression with a string value that contains seven fields\. In this case, you can use the first three fields to specify the time for when a scheduled action should be run, including the seconds\. The full cron expression has the following space\-separated fields: \[Seconds\] \[Minutes\] \[Hours\] \[Day\_of\_Month\] \[Month\] \[Day\_of\_Week\] \[Year\]\. However, this approach doesn't guarantee that the scheduled action will run on the precise second that you specify\. Also, some service consoles may not support the seconds field in a cron expression\.