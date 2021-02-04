# Application Auto Scaling monitoring<a name="monitoring-overview"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Application Auto Scaling and your other AWS solutions\. You should collect monitoring data from all parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides monitoring tools to watch Application Auto Scaling, report when something is wrong, and take automatic actions when appropriate\.

You can use the following features to help you manage your AWS resources:

**Amazon CloudWatch Alarms**  
To detect unhealthy application behavior, CloudWatch helps you by automatically monitoring certain metrics for your AWS resources\. You can configure a CloudWatch alarm and set up an Amazon SNS notification that sends an email when a metric’s value is not what you expect or when certain anomalies are detected\. For example, you can be notified when network activity is suddenly higher or lower than a metric's expected value\. For more information, see [Monitoring with CloudWatch alarms](cloudwatch-alarm-notifications.md) and the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

**Amazon CloudWatch Dashboards**  
Amazon CloudWatch monitors your AWS resources and the applications that you run on AWS in real time\. CloudWatch dashboards are customizable home pages in the CloudWatch console\. You can use these pages to monitor your resources in a single view, even including resources that are spread across different Regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. For more information, see [Building dashboards with CloudWatch](monitoring-cloudwatch.md)\.

**Amazon EventBridge**  
EventBridge delivers a near real time stream of system events that describe changes in AWS resources\. EventBridge enables automated event\-driven computing\. You can write rules that watch for certain events and trigger automated actions in other AWS services when these events happen\. For more information, see [Getting notified of events preventing scaling through EventBridge](monitoring-eventbridge.md)\.

**AWS CloudTrail**  
AWS CloudTrail captures API calls and related events made by or on behalf of your AWS account\. Then it delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\. For information about the Application Auto Scaling API calls that are logged by CloudTrail, see [Logging Application Auto Scaling API calls with CloudTrail](https://docs.aws.amazon.com/autoscaling/application/APIReference/logging-using-cloudtrail.html)\.

**Amazon CloudWatch Logs**  
Amazon CloudWatch Logs enable you to monitor, store, and access your log files from Amazon EC2 instances, CloudTrail, and other sources\. CloudWatch Logs can monitor information in the log files and notify you when certain thresholds are met\. You can also archive your log data in highly durable storage\. For more information, see the [Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\.

**AWS Personal Health Dashboard**  
The Personal Health Dashboard \(PHD\) displays information, and also provides notifications that are triggered by changes in the health of AWS resources\. The information is presented in two ways: on a dashboard that shows recent and upcoming events organized by category, and in a full event log that shows all events from the past 90 days\. For more information, see [Personal Health Dashboard notifications for Application Auto Scaling](monitoring-personal-health-dashboard.md)\.