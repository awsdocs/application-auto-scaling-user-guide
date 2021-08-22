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
+ SageMaker endpoint variants
+ Spot Fleet requests
+ Custom resources provided by your own applications or services\. For more information, see the [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\. 

To see the regional availability for any of the AWS services listed above, see the [Region table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

For information about scaling your fleet of Amazon EC2 instances using Auto Scaling groups, see the [Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)\. 

You can also use Application Auto Scaling and Amazon EC2 Auto Scaling in combination with AWS Auto Scaling to scale resources across multiple services\. AWS Auto Scaling can help you maintain optimal availability and performance by combining predictive scaling and dynamic scaling \(proactive and reactive approaches, respectively\) together to scale your Amazon EC2 capacity faster\. For more information, see the [AWS Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/plans/userguide/)\.

## Features of Application Auto Scaling<a name="features"></a>

Application Auto Scaling allows you to automatically scale your scalable resources according to conditions that you define\.
+ **Target tracking scaling**—Scale a resource based on a target value for a specific CloudWatch metric\.
+ **Step scaling**— Scale a resource based on a set of scaling adjustments that vary based on the size of the alarm breach\.
+ **Scheduled scaling**—Scale a resource based on the date and time\.

## Accessing Application Auto Scaling<a name="access"></a>

If you've signed up for an AWS account, access Application Auto Scaling by signing into the AWS Management Console\. Then, open the service console for one of the resources listed in the introduction\. Ensure that you open the console in the same AWS Region as the resource that you want to work with\. For more information, see [Getting started with Application Auto Scaling](getting-started.md)\.

You can also access Application Auto Scaling using the [Application Auto Scaling API](https://docs.aws.amazon.com/autoscaling/application/APIReference/)\. Application Auto Scaling provides a Query API\. These requests are HTTP or HTTPS requests that use the HTTP verbs GET or POST and a Query parameter named `Action`\. For more information, see [Actions](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_Operations.html) in the *Application Auto Scaling API Reference*\.

If you prefer to build applications using language\-specific APIs instead of submitting a request over HTTP or HTTPS, AWS provides libraries, sample code, tutorials, and other resources for software developers\. These libraries provide basic functions that automate tasks such as cryptographically signing your requests, retrying requests, and handling error responses, making it is easier for you to get started\. For more information, see [AWS SDKs and tools](https://aws.amazon.com/tools/)\.

If you prefer to use a command line interface, you have the following options:

**AWS Command Line Interface \(AWS CLI\)**  
Provides commands for a broad set of AWS products, and is supported on Windows, macOS, and Linux\. To get started, see [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For more information, see [application\-autoscaling](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/) in the *AWS CLI Command Reference*\.

**AWS Tools for Windows PowerShell**  
Provides commands for a broad set of AWS products for those who script in the PowerShell environment\. To get started, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information, see the [AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/)\.

Application Auto Scaling also supports configuring scaling using AWS CloudFormation\. For examples of JSON and YAML templates, see [Application Auto Scaling template examples](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-autoscaling.html#scenario-app-as-template-examples) in the AWS CloudFormation User Guide\. To provision and configure a stack of resources, you must first understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\.

For information about your credentials for accessing AWS, see [AWS security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *Amazon Web Services General Reference*\. For information about regions and endpoints for Application Auto Scaling, see the [Regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/as-app.html) table in the *AWS General Reference*\. 