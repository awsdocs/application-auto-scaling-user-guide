# Create a target tracking scaling policy for Application Auto Scaling using metric math<a name="application-auto-scaling-target-tracking-metric-math"></a>

Using metric math, you can query multiple CloudWatch metrics and use math expressions to create new time series based on these metrics\. You can visualize the resulting time series in the CloudWatch console and add them to dashboards\. For more information about metric math, see [Using metric math](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-metric-math.html) in the *Amazon CloudWatch User Guide*\. 

The following considerations apply to metric math expressions:
+ You can query any available CloudWatch metric\. Each metric is a unique combination of metric name, namespace, and zero or more dimensions\. 
+ You can use any arithmetic operator \(\+ \- \* / ^\), statistical function \(such as AVG or SUM\), or other function that CloudWatch supports\. 
+ You can use both metrics and the results of other math expressions in the formulas of the math expression\. 
+ Any expressions used in a metric specification must eventually return a single time series\.
+ You can verify that a metric math expression is valid by using the CloudWatch console or the CloudWatch [GetMetricData](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricData.html) API\.

**Note**  
You can create a target tracking scaling policy using metric math only if you use the AWS CLI or an SDK\. This feature is not yet available in AWS CloudFormation\.

**Topics**
+ [Example: Amazon SQS queue backlog per task](#metric-math-sqs-queue-backlog)
+ [Limitations](#metric-math-limitations)

## Example: Amazon SQS queue backlog per task<a name="metric-math-sqs-queue-backlog"></a>

To calculate the Amazon SQS queue backlog per task, take the approximate number of messages available for retrieval from the queue and divide that number by the number of Amazon ECS tasks running in the service\. For more information, see [Amazon Elastic Container Service \(ECS\) Auto Scaling using custom metrics](http://aws.amazon.com/blogs/containers/amazon-elastic-container-service-ecs-auto-scaling-using-custom-metrics/) on the AWS Compute Blog\.

The logic for the expression is this:

 `sum of (number of messages in the queue)/(number of tasks that are currently in the RUNNING state)`

Then your CloudWatch metric information is the following\.


| ID | CloudWatch metric | Statistic | Period | 
| --- | --- | --- | --- | 
| m1 | ApproximateNumberOfMessagesVisible | Sum | 1 minute | 
| m2 | RunningTaskCount | Average | 1 minute | 

Your metric math ID and expression are the following\.


| ID | Expression | 
| --- | --- | 
| e1 | \(m1\)/\(m2\) | 

**To use this metric math to create a target tracking scaling policy \(AWS CLI\)**

1. Store the metric math expression as part of a customized metric specification in a JSON file named `config.json`\. 

   Use the following example to help you get started\. Replace the *replaceable italicized values* with those that are appropriate for your application\.

   ```
   {
       "CustomizedMetricSpecification": {
           "Metrics": [
               {
                   "Label": "Get the queue size (the number of messages waiting to be processed)",
                   "Id": "m1",
                   "MetricStat": {
                       "Metric": {
                           "MetricName": "ApproximateNumberOfMessagesVisible",
                           "Namespace": "AWS/SQS",
                           "Dimensions": [
                               {
                                   "Name": "QueueName",
                                   "Value": "my-queue"
                               }
                           ]
                       },
                       "Stat": "Sum"
                   },
                   "ReturnData": false
               },
               {
                   "Label": "Get the ECS running task count (the number of currently running tasks)",
                   "Id": "m2",
                   "MetricStat": {
                       "Metric": {
                           "MetricName": "RunningTaskCount",
                           "Namespace": "ECS/ContainerInsights",
                           "Dimensions": [
                               {
                                   "Name": "ClusterName",
                                   "Value": "default"
                               },
                               {
                                   "Name": "ServiceName",
                                   "Value": "web-app"
                               }
                           ]
                       },
                       "Stat": "Average"
                   },
                   "ReturnData": false
               },
               {
                   "Label": "Calculate the backlog per instance",
                   "Id": "e1",
                   "Expression": "m1 / m2",
                   "ReturnData": true
               }
           ]
       },
       "TargetValue": 100
   }
   ```

   For more information, see [TargetTrackingScalingPolicyConfiguration](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_TargetTrackingScalingPolicyConfiguration.html) in the *Application Auto Scaling API Reference*\.
**Note**  
Following are some additional resources that can help you find metric names, namespaces, dimensions, and statistics for CloudWatch metrics:   
For information about the available metrics for AWS services, see [AWS services that publish CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html) in the *Amazon CloudWatch User Guide*\.
To get the exact metric name, namespace, and dimensions \(if applicable\) for a CloudWatch metric with the AWS CLI, see [list\-metrics](https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/list-metrics.html)\. 

1. To create this policy, run the [put\-scaling\-policy](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy) command using the JSON file as input, as demonstrated in the following example\.

   ```
   aws application-autoscaling put-scaling-policy --policy-name sqs-backlog-target-tracking-scaling-policy \
     --service-namespace ecs --scalable-dimension ecs:service:DesiredCount --resource-id service/default/web-app \
     --policy-type TargetTrackingScaling --target-tracking-scaling-policy-configuration file://config.json
   ```

   If successful, this command returns the policy's Amazon Resource Name \(ARN\) and the ARNs of the two CloudWatch alarms created on your behalf\.

   ```
   {
       "PolicyARN": "arn:aws:autoscaling:us-west-2:012345678910:scalingPolicy: 8784a896-b2ba-47a1-b08c-27301cc499a1:resource/ecs/service/default/web-app:policyName/sqs-backlog-target-tracking-scaling-policy",
       "Alarms": [
           {
               "AlarmARN": "arn:aws:cloudwatch:us-west-2:012345678910:alarm:TargetTracking-service/default/web-app-AlarmHigh-9bc77b56-0571-4276-ba0f-d4178882e0a0",
               "AlarmName": "TargetTracking-service/default/web-app-AlarmHigh-9bc77b56-0571-4276-ba0f-d4178882e0a0"
           },
           {
               "AlarmARN": "arn:aws:cloudwatch:us-west-2:012345678910:alarm:TargetTracking-service/default/web-app-AlarmLow-9b6ad934-6d37-438e-9e05-02836ddcbdc4",
               "AlarmName": "TargetTracking-service/default/web-app-AlarmLow-9b6ad934-6d37-438e-9e05-02836ddcbdc4"
           }
       ]
   }
   ```
**Note**  
If this command throws an error, make sure that you have updated the AWS CLI locally to the latest version\.

## Limitations<a name="metric-math-limitations"></a>
+ The maximum request size is 50 KB\. This is the total payload size for the [PutScalingPolicy](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_PutScalingPolicy.html) API request when you use metric math in the policy definition\. If you exceed this limit, Application Auto Scaling rejects the request\.
+ The following services are not supported when using metric math with target tracking scaling policies:
  + Amazon Keyspaces \(for Apache Cassandra\) 
  + DynamoDB
  + Amazon EMR
  + Amazon MSK
  + Amazon Neptune