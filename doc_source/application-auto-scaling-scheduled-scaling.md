# Scheduled Scaling for Application Auto Scaling<a name="application-auto-scaling-scheduled-scaling"></a>

Scaling based on a schedule allows you to set your own scaling schedule for predictable load changes\. For example, every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday\. You can configure Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday\.

To use scheduled scaling, create *scheduled actions*, which tell Application Auto Scaling to perform scaling activities at specific times\. When you create a scheduled action, you specify the scalable target, when the scaling activity should occur, and the minimum and maximum capacity\. At the specified time, Application Auto Scaling scales based on the new capacity values\.

Application Auto Scaling guarantees the order of execution for scheduled actions for the same scalable target but not for scheduled actions across scalable targets\.

## Using the AWS CLI to Create or Update a Scheduled Action<a name="create-scheduled-action"></a>

You can create scheduled actions that scale one time only or that scale on a recurring schedule using the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. When you specify the new capacity, you can specify a minimum capacity, a maximum capacity, or both\. 

**Note**  
For brevity, the examples in the sections below illustrate how to create scheduled actions for Amazon ECS, DynamoDB, Spot Fleet, and a custom resource\. To specify a different scalable target, specify its namespace in `--service-namespace`, its scalable dimension in `--scalable-dimension`, and its resource ID in `--resource-id`\. 

### Schedule Actions for a One\-Time Schedule<a name="one-time-schedule"></a>

You can specify a one\-time schedule to automatically scale your scalable target at a certain date and time, in UTC\. 

**Example Example: To scale in one time only**  
At the date and time specified for `--schedule`, if the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.  

```
aws application-autoscaling put-scheduled-action --service-namespace ecs \
--scalable-dimension ecs:service:DesiredCount \
--resource-id service/default/web-app \
--scheduled-action-name my-one-time-action \
--schedule "at(2019-01-31T17:00:00)" \
--scalable-target-action MaxCapacity=10
```

**Example Example: To scale out one time only**  
At the date and time specified for `--schedule`, if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\.  

```
aws application-autoscaling put-scheduled-action --service-namespace custom-resource \
--scalable-dimension custom-resource:ResourceType:Property \
--resource-id file://~/custom-resource-id.txt \
--scheduled-action-name my-one-time-action \
--schedule "at(2019-03-31T22:00:00)" \
--scalable-target-action MinCapacity=3
```

The contents of the `custom-resource-id.txt` file, which is specified for `--resource-id` in the example above:

**Example**  

```
https://example.execute-api.us-west-2.amazonaws.com/prod/scalableTargetDimensions/1-23456789
```

For more information about configuring a custom resource, see our [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\.

### Schedule Actions on a Recurring Schedule<a name="recurrence-schedule"></a>

You can specify a recurrence schedule for a scheduled action using the `--schedule` option of the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. Application Auto Scaling supports cron and rate formats for schedule expressions\. For more information, see [Cron Expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions) and [Rate Expressions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#RateExpressions) in the *Amazon CloudWatch Events User Guide*\.

**Example Example: To scale on a recurring schedule using a cron expression**  
On the specified schedule \(every day at 12:00pm UTC\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.  

```
aws application-autoscaling put-scheduled-action --service-namespace dynamodb \
--scalable-dimension dynamodb:table:WriteCapacityUnits \
--resource-id table/my-table \
--scheduled-action-name my-recurring-action \
--schedule "cron(0 12 * * ? *)" \
--scalable-target-action MinCapacity=10,MaxCapacity=50
```

**Example Example: To scale on a recurring schedule using a rate expression**  
On the specified schedule \(every hour\), if the value specified for `MinCapacity` is above the current capacity, Application Auto Scaling scales out to `MinCapacity`\. If the value specified for `MaxCapacity` is below the current capacity, Application Auto Scaling scales in to `MaxCapacity`\.   

```
aws application-autoscaling put-scheduled-action --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--scheduled-action-name my-recurring-action \
--schedule "rate(1 hour)" \
--scalable-target-action MinCapacity=3,MaxCapacity=10
```

## Describe Scheduled Actions<a name="describe-scheduled-actions"></a>

You can describe all scheduled actions for the specified service namespace using the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) command\.

```
aws application-autoscaling describe-scheduled-actions --service-namespace ec2
```

The following is example output\.

```
{
    "ScheduledActions": [
        {
            "ScheduledActionARN": "<arn>",
            "ServiceNamespace": "ec2",
            "CreationTime": 1515026382.218,
            "ScalableDimension": "ec2:spot-fleet-request:TargetCapacity",
            "Schedule": "at(2018-01-31T17:00:00)",
            "ScalableTargetAction": {
                "MaxCapacity": 3
            },
            "ScheduledActionName": "my-one-time-action",
            "ResourceId": "spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE"
        }
    ]
}
```

## Delete a Scheduled Action<a name="delete-scheduled-action"></a>

When you are finished with a scheduled action, you can delete it using the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\.

The following command deletes the specified scheduled action for the specified Spot Fleet request\.

```
aws application-autoscaling delete-scheduled-action --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--scheduled-action-name my-spot-fleet-action
```