# Scheduled Scaling<a name="application-auto-scaling-scheduled-scaling"></a>

Scaling based on a schedule enables you to scale your application in response to predictable load changes\. You can plan scaling activities based on the predictable traffic patterns of your web application\. For example, every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday\. You can configure Application Auto Scaling to increase capacity on Wednesday and decrease capacity on Friday\.

To use scheduled scaling, you create *scheduled actions*, which tell Application Auto Scaling to perform scaling activities at specific times\. When you create a scheduled action, you specify the scalable target, when the scaling activity should occur, the minimum capacity, and the maximum capacity\. At the specified time, Application Auto Scaling scales based on the new capacity values\.

Application Auto Scaling guarantees the order of execution for scheduled actions for the same scalable target but not for scheduled actions across scalable targets\.

## Create a Scheduled Action<a name="create-scheduled-action"></a>

You can create a scheduled action to scale one time only or on a recurring schedule using the [put\-scheduled\-action](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. When you specify the new capacity, you can specify a minimum capacity, a maximum capacity, or both a minimum and a maximum capacity\.

**Example Example: To scale one time only**  
At the date and time specified for `--schedule`, if the current capacity is above the value specified for `MaxCapacity`, Application Auto Scaling scales in to the value specified by `MaxCapacity`\.  

```
aws application-autoscaling put-scheduled-action --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--scheduled-action-name my-one-time-action \
--schedule at(2018-01-31T17:00:00) \
--scalable-target-action MaxCapacity=3
```

**Example Example: To scale on a recurring schedule**  
On the specified schedule, if the current capacity is below the value specified for `MinCapacity`, Application Auto Scaling scales out to the value specified by `MinCapacity`\.  

```
aws application-autoscaling put-scheduled-action --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--scheduled-action-name my-recurring-action \
--schedule cron(expression) \
--scalable-target-action MinCapacity=10
```

## Describe Scheduled Actions<a name="describe-scheduled-actions"></a>

You can describe all scheduled actions for the specified service namespace using the following [describe\-scheduled\-actions](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) command\.

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

When you are finished with a scheduled action, you can delete it using the [delete\-scheduled\-action](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\.

The following command deletes the specified scheduled action for the specified Spot fleet request\.

```
aws application-autoscaling delete-scheduled-action --service-namespace ec2 \
--scalable-dimension ec2:spot-fleet-request:TargetCapacity \
--resource-id spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE \
--scheduled-action-name my-spot-fleet-action
```