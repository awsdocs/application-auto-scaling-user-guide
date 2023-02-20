# Monitor Application Auto Scaling events with EventBridge<a name="monitoring-eventbridge"></a>

Amazon EventBridge, formerly called CloudWatch Events, helps you monitor events that are specific to Application Auto Scaling and initiate target actions that use other AWS services\. Events from AWS services are delivered to EventBridge in near real time\. 

Using EventBridge, you can create *rules* that match incoming *events* and route them to *targets* for processing\. 

For more information, see [Getting started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

## Application Auto Scaling events<a name="application-auto-scaling-example-event-types"></a>

The following examples show events for Application Auto Scaling\. Events are produced on a best\-effort basis\.

Only events that are specific to scaled to max and API calls via CloudTrail are currently available for Application Auto Scaling\.

**Topics**
+ [Event for state change: scaled to max](#event-example-1)
+ [Events for API calls via CloudTrail](#event-example-2)

### Event for state change: scaled to max<a name="event-example-1"></a>

The following example event shows that Application Auto Scaling increased \(scaled out\) the capacity of the scalable target to its maximum size limit\. If demand increases again, Application Auto Scaling will be prevented from scaling the target to a larger size because it is already scaled to its maximum size\. 

In the `detail` object, the values for the `resourceId`, `serviceNamespace`, and `scalableDimension` attributes identify the scalable target\. The values for the `newDesiredCapacity` and `oldDesiredCapacity` attributes refer to the new capacity after the scale\-out event and the original capacity before the scale\-out event\. The `maxCapacity` is the maximum size limit of the scalable target\. 

```
{
  "version": "0",
  "id": "11112222-3333-4444-5555-666677778888",
  "detail-type": "Application Auto Scaling Scaling Activity State Change",
  "source": "aws.application-autoscaling",
  "account": "123456789012",
  "time": "2019-06-12T10:23:40Z",
  "region": "us-west-2",
  "resources": [],
  "detail": {
    "startTime": "2022-06-12T10:20:43Z",
    "endTime": "2022-06-12T10:23:40Z",
    "newDesiredCapacity": 8,
    "oldDesiredCapacity": 5,
    "minCapacity": 2,
    "maxCapacity": 8,
    "resourceId": "table/my-table",
    "scalableDimension": "dynamodb:table:WriteCapacityUnits",
    "serviceNamespace": "dynamodb",
    "statusCode": "Successful",
    "scaledToMax": true,
    "direction": "scale-out"
}
```

To create a rule that captures all `scaledToMax` state change events for all scalable targets, use the following sample event pattern\.

```
{
  "source": [
    "aws.application-autoscaling"
  ],
  "detail-type": [
    "Application Auto Scaling Scaling Activity State Change"
  ],
  "detail": {
    "scaledToMax": [
      true 
    ]
  } 
}
```

### Events for API calls via CloudTrail<a name="event-example-2"></a>

A *trail* is a configuration that AWS CloudTrail uses to deliver events as log files to an Amazon S3 bucket\. CloudTrail log files contain log entries\. An event represents a log entry, and it includes information about the requested action, the date and time of the action, and request parameters\. To learn how to get started with CloudTrail, see [Creating a trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html) in the *AWS CloudTrail User Guide*\.

Events that are delivered via CloudTrail have `AWS API Call via CloudTrail` as the value for `detail-type`\. 

The following example event represents a CloudTrail log file entry that shows that a console user called the Application Auto Scaling [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) action\.

```
{
  "version": "0",
  "id": "99998888-7777-6666-5555-444433332222",
  "detail-type": "AWS API Call via CloudTrail",
  "source": "aws.autoscaling",
  "account": "123456789012",
  "time": "2022-07-13T16:50:15Z",
  "region": "us-west-2",
  "resources": [],
  "detail": {
    "eventVersion": "1.08",
    "userIdentity": {
      "type": "IAMUser",
      "principalId": "123456789012",
      "arn": "arn:aws:iam::123456789012:user/Bob",
      "accountId": "123456789012",
      "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "sessionContext": {
        "sessionIssuer": {
          "type": "Role",
          "principalId": "123456789012",
          "arn": "arn:aws:iam::123456789012:role/Admin",
          "accountId": "123456789012",
          "userName": "Admin"
        },
        "webIdFederationData": {},
        "attributes": {
          "creationDate": "2022-07-13T15:17:08Z",
          "mfaAuthenticated": "false"
        }
      }
    },
    "eventTime": "2022-07-13T16:50:15Z",
    "eventSource": "autoscaling.amazonaws.com",
    "eventName": "RegisterScalableTarget",
    "awsRegion": "us-west-2",
    "sourceIPAddress": "AWS Internal",
    "userAgent": "EC2 Spot Console",
    "requestParameters": {
      "resourceId": "spot-fleet-request/sfr-73fbd2ce-aa30-494c-8788-1cee4EXAMPLE",
      "serviceNamespace": "ec2",
      "scalableDimension": "ec2:spot-fleet-request:TargetCapacity",
      "minCapacity": 2,
      "maxCapacity": 10
    },
    "responseElements": null,
    "additionalEventData": {
      "service": "application-autoscaling"
    },
    "requestID": "e9caf887-8d88-11e5-a331-3332aa445952",
    "eventID": "49d14f36-6450-44a5-a501-b0fdcdfaeb98",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "123456789012",
    "eventCategory": "Management",
    "sessionCredentialFromConsole": "true"
  }
}
```

To create a rule based on all [DeleteScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeleteScalingPolicy.html) and [DeregisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_DeregisterScalableTarget.html) API calls for all scalable targets, use the following sample event pattern:

```
{
  "source": [
    "aws.autoscaling"
  ],
  "detail-type": [
    "AWS API Call via CloudTrail"
  ],
  "detail": {
    "eventSource": [
      "autoscaling.amazonaws.com"
    ],
    "eventName": [
       "DeleteScalingPolicy", 
       "DeregisterScalableTarget"
    ],
    "additionalEventData": {
      "service": [
        "application-autoscaling"
      ]
    }
  } 
}
```

For more information about using CloudTrail, see [Logging Application Auto Scaling API Calls with AWS CloudTrail](https://docs.aws.amazon.com/autoscaling/application/APIReference/logging-using-cloudtrail.html) in the *Application Auto Scaling API Reference*\.