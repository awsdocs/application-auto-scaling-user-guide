# Monitoring with CloudWatch alarms<a name="cloudwatch-alarm-notifications"></a>

You can create alarms to notify you when Amazon CloudWatch has detected any problems that might require your attention\. CloudWatch helps you by automatically monitoring certain metrics for AWS services\. 

A CloudWatch alarm watches a single metric\. It invokes one or more actions only when the alarm state changes, and has persisted for the period that you specify\. For example, you can set an alarm that notifies you when a metric value falls to or exceeds a certain level, ensuring that you are notified before a potential problem occurs\.

CloudWatch also allows you to set an alarm that notifies you when the metric is in `INSUFFICIENT_DATA` state\. Any metric, for any AWS service, can alarm on `INSUFFICIENT_DATA`\. This is the initial state of a new alarm, but the alarm state also changes to `INSUFFICIENT_DATA` if CloudWatch metrics become unavailable, or not enough data is available for the metric to determine the alarm state\. For example, AWS Lambda emits the `ProvisionedConcurrencyUtilization` metric to CloudWatch every minute only when the Lambda function is active\. If the function is inactive, this results in the alarm going to the `INSUFFICIENT_DATA` state while waiting for the metrics\. This is normal and might not necessarily mean that there is a problem, but it could be indicative of a problem if you expected activity within a period of time but there was none\.

This topic explains how to create an alarm that sends a notification when the metric is within or outside a threshold that you define, or when there is insufficient data\. For more detailed information about alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

**To create an alarm that sends email**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, **Create Alarm**\.

1. Choose **Select Metric**\.

   You are directed to a page where you can find all of your metrics\. The types of metrics available to you depends on the services and features that you use\. Metrics are grouped first by the service namespace, then by the various dimension combinations within each namespace\.

1. Select a metric namespace \(for example, **Lambda**\) and then a metric dimension \(for example, **By Function Name**\)\. 

   The **All metrics** tab displays all metrics for the selected dimension and namespace\.

1. Select the check box next to the metric that you want to create an alarm for, and then choose **Select metric**\.

1. Configure the alarm as follows, and then choose **Next**:
   + Under **Metric**, select an aggregation period of `1 minute` or `5 minutes`\. If you use one minute as an aggregation period for a metric, there will be one data point every minute\. The shorter period creates a more sensitive alarm\.
   + Under **Conditions**, configure your threshold, for example, the value that the metric must exceed before a notification is generated\.
   + Under **Additional configuration**, for **Datapoints to alarm**, enter the number of data points \(evaluation periods\) during which the metric value must meet the threshold conditions to trigger the alarm\. For example, two consecutive periods of 5 minutes would take 10 minutes to trigger the alarm\.
   + For **Missing data treatment**, keep the default and treat missing data points as missing\.

     Some metrics are reported only when there is activity occurring\. This can result in a sparsely reported metric\. If a metric is frequently missing data points by design, the state of the alarm is `INSUFFICIENT_DATA` during those periods\. To force the alarm to maintain the previous `ALARM` or `OK` state to keep alerts from flapping, you could choose to ignore missing data instead\.

1. Under **Notification**, choose or create an SNS topic to notify when the alarm is in `ALARM` state, `OK` state, or `INSUFFICIENT_DATA` state\. To have the alarm send multiple notifications for the same alarm state or for different alarm states, choose **Add notification**\. 

1. When finished, choose **Next**\. 

1. Enter a name and, optionally, a description for the alarm, and then choose **Next**\.

1. Choose **Create alarm**\. 

**To check the state of your alarms**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms** to see a list of alarms\.

1. To filter alarms, use the drop\-down filters next to the search field, and choose the filter option that you want to apply\.

1. To edit or delete an alarm, select the alarm and then choose **Actions**, **Edit** or **Actions**, **Delete**\.