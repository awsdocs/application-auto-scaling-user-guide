# What Is Application Auto Scaling?<a name="what-is-application-auto-scaling"></a>

Application Auto Scaling is a web service for developers and system administrators who need a solution for automatically scaling their scalable resources for individual AWS services beyond Amazon EC2\. Application Auto Scaling allows you to configure automatic scaling for the following resources: 
+ Amazon ECS services
+ Spot Fleet requests
+ Amazon EMR clusters
+ AppStream 2\.0 fleets
+ DynamoDB tables and global secondary indexes
+ Aurora replicas
+ Amazon SageMaker endpoint variants
+ Custom resources provided by your own applications or services\. For more information, see the [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)\. 

You have several options for scaling with AWS\. For information about scaling your fleet of Amazon EC2 instances, see the [Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)\. 

You can also use Application Auto Scaling and Amazon EC2 Auto Scaling in combination with AWS Auto Scaling to scale resources across multiple services\. AWS Auto Scaling can help you maintain optimal availability and performance by combining predictive scaling and dynamic scaling \(proactive and reactive approaches, respectively\) together to scale your Amazon EC2 capacity faster\. For more information, see the [AWS Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/plans/userguide/)\.

## Features of Application Auto Scaling<a name="features"></a>

Application Auto Scaling allows you to automatically scale your scalable resources according to conditions that you define\.
+ **Target tracking scaling**—Scale a resource based on a target value for a specific CloudWatch metric\.
+ **Step scaling**— Scale a resource based on a set of scaling adjustments that vary based on the size of the alarm breach\.
+ **Scheduled scaling**—Scale a resource based on the date and time\.

## Getting Started<a name="getting-started"></a>

Application Auto Scaling integrates with all of the following services, so that you can call API actions directly from the console of the resources that you want to scale\. If you are a first\-time user of Application Auto Scaling, we recommend that you refer to the following documentation for the services you’re interested in to learn more about how they integrate with Application Auto Scaling\. These topics contain information that's especially helpful for users who mainly interact with Application Auto Scaling by using the AWS Management Console\.
+ [Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) in the *Amazon Elastic Container Service Developer Guide*
+ [Automatic Scaling for Spot Fleet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-automatic-scaling.html) in the *Amazon EC2 User Guide*
+ [Using Automatic Scaling in Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-automatic-scaling.html) in the *Amazon EMR Management Guide*
+ [Fleet Auto Scaling for AppStream 2\.0](https://docs.aws.amazon.com/appstream2/latest/developerguide/autoscaling.html) in the *Amazon AppStream 2\.0 Developer Guide*
+ [Managing Throughput Capacity with DynamoDB Auto Scaling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html) in the *Amazon DynamoDB Developer Guide*
+ [Using Amazon Aurora Auto Scaling with Aurora Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Integrating.AutoScaling.html) in the *Amazon RDS User Guide*
+ [Automatically Scaling Amazon SageMaker Models](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html) in the *Amazon SageMaker Developer Guide*

To see the regional availability for any of the AWS services listed above, see the [AWS Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\.

If you would like the Application Auto Scaling API actions to be specified using command line options, you also have option of using this user guide\. To get started, complete the exercises in [Getting Started Using the AWS CLI](get-started-exercise.md)\. In this tutorial, we show you how to use the AWS Command Line Interface \(AWS CLI\) to programmatically access Application Auto Scaling\. However, if, at any time, you need information not included in this user guide, including examples of scaling policies that you can try, see the above service documentation\. 

## Accessing Application Auto Scaling<a name="access"></a>

If you've signed up for an AWS account, access Application Auto Scaling by signing into the AWS Management Console\. Then, open the service console for one of the services listed in the Getting Started section\.

You can also access Application Auto Scaling using the [Application Auto Scaling API](https://docs.aws.amazon.com/autoscaling/application/APIReference/)\. Application Auto Scaling provides a Query API\. These requests are HTTP or HTTPS requests that use the HTTP verbs GET or POST and a Query parameter named `Action`\. For more information, see [Actions](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_Operations.html) in the *Application Auto Scaling API Reference*\.

If you prefer to build applications using language\-specific APIs instead of submitting a request over HTTP or HTTPS, AWS provides libraries, sample code, tutorials, and other resources for software developers\. These libraries provide basic functions that automate tasks such as cryptographically signing your requests, retrying requests, and handling error responses, making it is easier for you to get started\. For more information, see [AWS SDKs and Tools](https://aws.amazon.com/tools/)\.

If you prefer to use a command line interface, you have the following options:

**AWS Command Line Interface \(AWS CLI\)**  
Provides commands for a broad set of AWS products, and is supported on Windows, macOS, and Linux\. To get started, see [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For more information, see [application\-autoscaling](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/) in the *AWS CLI Command Reference*\.

**AWS Tools for Windows PowerShell**  
Provides commands for a broad set of AWS products for those who script in the PowerShell environment\. To get started, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\. For more information, see the [AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/)\.

For information about your credentials for accessing AWS, see [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *Amazon Web Services General Reference*\. For information about regions and endpoints for Application Auto Scaling, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#autoscaling_region) in the *AWS General Reference*\. 