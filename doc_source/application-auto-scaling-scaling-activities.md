# Scaling activities for Application Auto Scaling<a name="application-auto-scaling-scaling-activities"></a>

Application Auto Scaling monitors your scaling policy's CloudWatch metrics and initiates a scaling activity when thresholds are exceeded\. It also initiates scaling activities when you modify the maximum or minimum size of the scalable target, either manually or following a schedule\.

When a scaling activity occurs, Application Auto Scaling does one of the following:
+ Increases the capacity of the scalable target \(referred to as *scaling out*\)
+ Decreases the capacity of the scalable target \(referred to as *scaling in*\)

You can look up scaling activities from the last six weeks\. 

## Looking up scaling activities by scalable target<a name="look-up-scaling-activities-with-the-aws-cli"></a>

To see the scaling activities for a specific scalable target, use the following [describe\-scaling\-activities](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) command\.

**Linux, macOS, or Unix**

```
aws application-autoscaling describe-scaling-activities --service-namespace ecs \
  --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app
```

**Windows**

```
aws application-autoscaling describe-scaling-activities --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app
```

The following is an example response, where `StatusCode` contains the current status of the activity and `StatusMessage` contains information about the status of the scaling activity\. 

```
{
    "ScalingActivities": [
        {
            "ScalableDimension": "ecs:service:DesiredCount",
            "Description": "Setting desired count to 1.",
            "ResourceId": "service/default/web-app",
            "ActivityId": "e6c5f7d1-dbbb-4a3f-89b2-51f33e766399",
            "StartTime": 1462575838.171,
            "ServiceNamespace": "ecs",
            "EndTime": 1462575872.111,
            "Cause": "monitor alarm web-app-cpu-lt-25 in state ALARM triggered policy web-app-cpu-lt-25",
            "StatusMessage": "Successfully set desired count to 1. Change successfully fulfilled by ecs.",
            "StatusCode": "Successful"
        }
    ]
}
```

For a description of the fields in the response, see [ScalingActivity](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html) in the *Application Auto Scaling API Reference*\. 

The following status codes indicate when the scaling event that leads to the scaling activity reaches a completed state:
+ `Successful` – Scaling was completed successfully
+ `Overridden` – The desired capacity was updated by a newer scaling event
+ `Unfulfilled` – Scaling timed out or the target service cannot fulfill the request
+ `Failed` – Scaling failed with an exception 

**Note**  
The scaling activity might also have a status of `Pending` or `InProgress`\. All scaling activities have a `Pending` status before the target service responds\. After the target responds, the status of the scaling activity changes to `InProgress`\.

## Including not scaled activities<a name="include-not-scaled-activities-with-the-aws-cli"></a>

By default, the scaling activities do not reflect times when Application Auto Scaling makes a decision about whether to not scale\.

For example, suppose that an Amazon ECS service exceeds the maximum threshold of a given metric, but the number of tasks is already at the maximum number of allowed tasks\. In this case, Application Auto Scaling does not scale out the desired number of tasks\.

To include activities that aren't scaled \(*not scaled activities*\) in the response, add the `--include-not-scaled-activities` option to the [describe\-scaling\-activities](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) command\. 

**Linux, macOS, or Unix**

```
aws application-autoscaling describe-scaling-activities --include-not-scaled-activities \
  --service-namespace ecs --scalable-dimension ecs:service:DesiredCount \
  --resource-id service/default/web-app
```

**Windows**

```
aws application-autoscaling describe-scaling-activities --include-not-scaled-activities --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app 
```

**Note**  
If this command throws an error, make sure that you have updated the AWS CLI locally to the latest version\.

To confirm that the response includes the not scaled activities, the `NotScaledReasons` element is shown in the output for some, if not all, failed scaling activities\. 

```
{
    "ScalingActivities": [
        {
            "ScalableDimension": "ecs:service:DesiredCount",
            "Description": "Attempting to scale due to alarm triggered",
            "ResourceId": "service/default/web-app",
            "ActivityId": "4d759079-a31f-4d0c-8468-504c56e2eecf",
            "StartTime": 1664928867.915,
            "ServiceNamespace": "ecs",
            "Cause": "monitor alarm web-app-cpu-gt-75 in state ALARM triggered policy web-app-cpu-gt-75",
            "StatusCode": "Failed",
            "NotScaledReasons": [ 
                { 
                    "Code": "AlreadyAtMaxCapacity",
                    "MaxCapacity": 4
                }
            ]
        }
    ]
}
```

For a description of the fields in the response, see [ScalingActivity](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_ScalingActivity.html) in the *Application Auto Scaling API Reference*\.

If a not scaled activity is returned, depending on the reason code listed in `Code`, attributes like `CurrentCapacity`, `MaxCapacity`, and `MinCapacity` might be present in the response\. 

## Understanding not scaled reason codes<a name="understand-not-scaled-reason-codes"></a>

The following are the reason codes for a not scaled activity\.


| Reason code | Definition | 
| --- | --- | 
| AutoScalingAnticipatedFlapping | Auto scaling algorithm decided not to take a scaling action because it would lead to flapping\. Flapping is an infinite loop of scaling in and scaling out\. That is, if a scaling action is taken, the metric value would change to start another scaling action in the reverse direction\.  | 
| TargetServicePutResourceAsUnscalable | The target service has temporarily put the resource in an unscalable state\. Application Auto Scaling will retry if the auto scaling conditions configured in the scaling policy are met\. | 
| AlreadyAtMaxCapacity | Scaling is blocked by the maximum capacity that you specified\. If you want Application Auto Scaling to scale out, you need to increase the maximum capacity\. | 
| AlreadyAtMinCapacity | Scaling is blocked by the minimum capacity that you specified\. If you want Application Auto Scaling to scale in, you need to decrease the minimum capacity\. | 
| AlreadyAtDesiredCapacity | Auto scaling algorithm calculated the revised capacity to be equal to the current capacity\.  | 