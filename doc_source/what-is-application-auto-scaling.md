# What Is Application Auto Scaling?<a name="what-is-application-auto-scaling"></a>

Application Auto Scaling allows you to configure automatic scaling for the following resources: 
+ Amazon ECS services
+ Spot Fleet requests
+ Amazon EMR clusters
+ AppStream 2\.0 fleets
+ DynamoDB tables and global secondary indexes
+ Aurora replicas
+ Amazon SageMaker endpoint variants
+ Custom resources provided by your own applications or services\. More information is available in our [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\. 

You have several options for scaling with AWS\. For information on scaling your fleet of Amazon EC2 instances, see the [Amazon EC2 Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/ec2/userguide/)\. You can also use Application Auto Scaling and Amazon EC2 Auto Scaling in combination with AWS Auto Scaling to scale resources across multiple services\. For more information, see the [AWS Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/plans/userguide/)\. 

## Features of Application Auto Scaling<a name="features"></a>

Application Auto Scaling allows you to automatically scale your scalable resources according to conditions that you define\.
+ **Target tracking scaling**—Scale a resource based on a target value for a specific CloudWatch metric\.
+ **Step scaling**— Scale a resource based on a set of scaling adjustments that vary based on the size of the alarm breach\.
+ **Scheduled scaling**—Scale a resource based on the date and time\.

## Accessing Application Auto Scaling<a name="access"></a>

AWS provides a web\-based user interface, the AWS Management Console\. If you've signed up for an AWS account, you can access Application Auto Scaling by signing into the AWS Management Console and opening the service console for your scalable resource\.

If you prefer to use a command line interface, you have the following options:

**AWS Command Line Interface \(AWS CLI\)**  
Provides commands for a broad set of AWS products, and is supported on Windows, macOS, and Linux\. To get started, see [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)\. For more information about the commands for Application Auto Scaling, see [application\-autoscaling](http://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/) in the *AWS CLI Command Reference*\.

**AWS Tools for Windows PowerShell**  
Provides commands for a broad set of AWS products for those who script in the PowerShell environment\. To get started, see the [AWS Tools for Windows PowerShell User Guide](http://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information about the cmdlets for Application Auto Scaling, see the [AWS Tools for PowerShell Cmdlet Reference](http://docs.aws.amazon.com/powershell/latest/reference/)\.

Application Auto Scaling provides a Query API\. These requests are HTTP or HTTPS requests that use the HTTP verbs GET or POST and a Query parameter named `Action`\. For more information about the API actions for Application Auto Scaling, see [Actions](http://docs.aws.amazon.com/autoscaling/application/APIReference/API_Operations.html) in the [Application Auto Scaling API Reference](http://docs.aws.amazon.com/autoscaling/application/APIReference/)\. 

If you prefer to build applications using language\-specific APIs instead of submitting a request over HTTP or HTTPS, AWS provides libraries, sample code, tutorials, and other resources for software developers\. These libraries provide basic functions that automate tasks such as cryptographically signing your requests, retrying requests, and handling error responses, making it is easier for you to get started\. For more information, see [AWS SDKs and Tools](http://aws.amazon.com/tools/)\.

For information about your credentials for accessing AWS, see [AWS Security Credentials](http://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *Amazon Web Services General Reference*\.

## Getting Started<a name="getting-started"></a>

To learn more about the AWS services supported by Application Auto Scaling, see the following documentation:
+ [Service Auto Scaling](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) in the *Amazon Elastic Container Service Developer Guide*
+ [Automatic Scaling for Spot Fleet](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-automatic-scaling.html) in the *Amazon EC2 User Guide*
+ [Using Automatic Scaling in Amazon EMR](http://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-automatic-scaling.html) in the *Amazon EMR Management Guide*
+ [Fleet Auto Scaling for AppStream 2\.0](http://docs.aws.amazon.com/appstream2/latest/developerguide/autoscaling.html) in the *Amazon AppStream 2\.0 Developer Guide*
+ [Managing Throughput Capacity with DynamoDB Auto Scaling](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html) in the *Amazon DynamoDB Developer Guide*
+ [Using Amazon Aurora Auto Scaling with Aurora Replicas](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Aurora.Integrating.AutoScaling.html) in the *Amazon RDS User Guide*
+ [Automatically Scaling Amazon SageMaker Models](http://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html) in the *Amazon SageMaker Developer Guide*

To see the regional availability for specific AWS services, see the [AWS Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\. 

For information about regions and endpoints for calls to the Application Auto Scaling API or CLI, visit [AWS Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#as-app_region) in the *AWS General Reference*\.