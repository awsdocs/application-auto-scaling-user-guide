# What is Application Auto Scaling?<a name="what-is-application-auto-scaling"></a>

Application Auto Scaling is a web service for developers and system administrators who need a solution for automatically scaling their scalable resources for individual AWS services beyond Amazon EC2\. Application Auto Scaling allows you to configure automatic scaling for the following resources:

 
+ AppStream 2\.0 fleets
+ Aurora replicas
+ Amazon Comprehend document classification and entity recognizer endpoints
+ DynamoDB tables and global secondary indexes
+ Amazon Elastic Container Service \(ECS\) services
+ ElastiCache for Redis clusters \(replication groups\)
+ Amazon EMR clusters
+ Amazon Keyspaces \(for Apache Cassandra\) tables
+ Lambda function provisioned concurrency
+ Amazon Managed Streaming for Apache Kafka \(MSK\) broker storage
+ Amazon Neptune clusters
+ SageMaker endpoint variants
+ Spot Fleet requests
+ Custom resources provided by your own applications or services\. For more information, see the [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\. 

To see the regional availability for any of the AWS services listed above, see the [Region table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

For information about scaling your fleet of Amazon EC2 instances using Auto Scaling groups, see the [Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)\. 

You can also use AWS Auto Scaling to create scaling plans to scale resources across multiple services\. For more information, see the [AWS Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/plans/userguide/)\.

## Features of Application Auto Scaling<a name="features"></a>

Application Auto Scaling allows you to automatically scale your scalable resources according to conditions that you define\.
+ **Target tracking scaling** – Scale a resource based on a target value for a specific CloudWatch metric\.
+ **Step scaling** – Scale a resource based on a set of scaling adjustments that vary based on the size of the alarm breach\.
+ **Scheduled scaling** – Scale a resource one time only or on a recurring schedule\.

## Work with Application Auto Scaling<a name="access"></a>

You can configure scaling using the following interfaces depending on the resource that you are scaling:
+ **AWS Management Console** – Provides a web interface that you can use to configure scaling\. If you've signed up for an AWS account, access Application Auto Scaling by signing into the AWS Management Console\. Then, open the service console for one of the resources listed in the introduction\. Ensure that you open the console in the same AWS Region as the resource that you want to work with\.
**Note**  
Console access is not available for all resources\. For more information, see [AWS services that you can use with Application Auto Scaling](integrated-services-list.md)\.
+ **AWS Command Line Interface \(AWS CLI\)** – Provides commands for a broad set of AWS services, and is supported on Windows, macOS, and Linux\. To get started, see [Set up the AWS CLI](setup-awscli.md)\. For more information, see [application\-autoscaling](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/index.html) in the *AWS CLI Command Reference*\.
+ **AWS Tools for Windows PowerShell** – Provides commands for a broad set of AWS products for those who script in the PowerShell environment\. To get started, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information, see the [AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/Index.html)\.
+ **AWS SDKs** – Provides language\-specific API operations and takes care of many of the connection details, such as calculating signatures, handling request retries, and handling errors\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.
+ **Query API** – Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access AWS services\. However, it requires your application to handle low\-level details such as generating the hash to sign the request, and handling errors\. For more information, see the [Application Auto Scaling API Reference](https://docs.aws.amazon.com/autoscaling/application/APIReference/)\.
+ **AWS CloudFormation** – Supports configuring scaling using a CloudFormation template\. For more information, see [Creating Application Auto Scaling resources with AWS CloudFormation](creating-resources-with-cloudformation.md)\.

To connect programmatically to an AWS service, you use an endpoint\. For information about endpoints for calls to Application Auto Scaling, see [Application Auto Scaling endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/as-app.html) in the *AWS General Reference*\. 