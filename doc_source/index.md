# Application Auto Scaling User Guide

-----
*****Copyright &copy; Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in
connection with any product or service that is not Amazon's,
in any manner that is likely to cause confusion among customers,
or in any manner that disparages or discredits Amazon. All other
trademarks not owned by Amazon are the property of their respective
owners, who may or may not be affiliated with, connected to, or
sponsored by Amazon.

-----
## Contents
+ [What is Application Auto Scaling?](what-is-application-auto-scaling.md)
+ [Setting up](setting-up.md)
   + [Set up the AWS CLI](setup-awscli.md)
   + [Use AWS CloudShell to work with Application Auto Scaling from the command line](use-awscli-with-cloudshell.md)
+ [Getting started with Application Auto Scaling](getting-started.md)
+ [AWS services that you can use with Application Auto Scaling](integrated-services-list.md)
   + [Amazon AppStream 2.0 and Application Auto Scaling](services-that-can-integrate-appstream.md)
   + [Amazon Aurora and Application Auto Scaling](services-that-can-integrate-aurora.md)
   + [Amazon Comprehend and Application Auto Scaling](services-that-can-integrate-comprehend.md)
   + [Amazon DynamoDB and Application Auto Scaling](services-that-can-integrate-dynamodb.md)
   + [Amazon ECS and Application Auto Scaling](services-that-can-integrate-ecs.md)
   + [ElastiCache for Redis and Application Auto Scaling](services-that-can-integrate-elasticache.md)
   + [Amazon Keyspaces (for Apache Cassandra) and Application Auto Scaling](services-that-can-integrate-keyspaces.md)
   + [AWS Lambda and Application Auto Scaling](services-that-can-integrate-lambda.md)
   + [Amazon Managed Streaming for Apache Kafka (MSK) and Application Auto Scaling](services-that-can-integrate-msk.md)
   + [Amazon Neptune and Application Auto Scaling](services-that-can-integrate-neptune.md)
   + [Amazon SageMaker and Application Auto Scaling](services-that-can-integrate-sagemaker.md)
   + [Amazon EC2 Spot Fleet and Application Auto Scaling](services-that-can-integrate-ec2.md)
   + [Custom resources and Application Auto Scaling](services-that-can-integrate-custom.md)
+ [Scheduled scaling for Application Auto Scaling](application-auto-scaling-scheduled-scaling.md)
   + [Schedule recurring scaling actions using cron expressions](scheduled-scaling-using-cron-expressions.md)
   + [Example scheduled actions for Application Auto Scaling](examples-scheduled-actions.md)
   + [Manage scheduled scaling for Application Auto Scaling](scheduled-scaling-additional-cli-commands.md)
   + [Tutorial: Getting started with scheduled scaling using the AWS CLI](get-started-exercise.md)
+ [Target tracking scaling policies for Application Auto Scaling](application-auto-scaling-target-tracking.md)
   + [Creating a target tracking scaling policy using the AWS CLI](create-target-tracking-policy-cli.md)
+ [Step scaling policies for Application Auto Scaling](application-auto-scaling-step-scaling-policies.md)
   + [Creating a step scaling policy using the AWS CLI](create-step-scaling-policy-cli.md)
+ [Tutorial: Configuring auto scaling to handle a heavy workload](application-auto-scaling-tutorial.md)
+ [Suspending and resuming scaling for Application Auto Scaling](application-auto-scaling-suspend-resume-scaling.md)
+ [Scaling activities for Application Auto Scaling](application-auto-scaling-scaling-activities.md)
+ [Application Auto Scaling monitoring](monitoring-overview.md)
   + [Monitoring with CloudWatch alarms](cloudwatch-alarm-notifications.md)
   + [Building dashboards with CloudWatch](monitoring-cloudwatch.md)
   + [Monitor Application Auto Scaling events with EventBridge](monitoring-eventbridge.md)
   + [AWS Health Dashboard notifications for Application Auto Scaling](monitoring-personal-health-dashboard.md)
+ [Security in Application Auto Scaling](security.md)
   + [Application Auto Scaling and interface VPC endpoints](application-auto-scaling-vpc-endpoints.md)
   + [Application Auto Scaling and data protection](application-auto-scaling-data-protection.md)
   + [Identity and Access Management for Application Auto Scaling](auth-and-access-control.md)
      + [How Application Auto Scaling works with IAM](security_iam_service-with-iam.md)
      + [AWS managed policies for Application Auto Scaling](security-iam-awsmanpol.md)
      + [Service-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)
      + [Application Auto Scaling identity-based policy examples](security_iam_id-based-policy-examples.md)
      + [Troubleshooting access to Application Auto Scaling](security_iam_troubleshoot.md)
      + [Permissions validation for API calls on target resources](security_iam_permission_validation.md)
   + [Compliance validation for Application Auto Scaling](application-auto-scaling-compliance.md)
   + [Resilience in Application Auto Scaling](disaster-recovery-resiliency.md)
   + [Infrastructure security in Application Auto Scaling](infrastructure-security.md)
+ [Quotas for Application Auto Scaling](application-auto-scaling-quotas.md)
+ [Creating Application Auto Scaling resources with AWS CloudFormation](creating-resources-with-cloudformation.md)
+ [Document history](doc-history.md)