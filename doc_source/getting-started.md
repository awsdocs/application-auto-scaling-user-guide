# Getting started with Application Auto Scaling<a name="getting-started"></a>

Application Auto Scaling integrates with other AWS services so that you can add scaling capabilities to meet your application's demand\. Automatic scaling is an optional feature of the service that is disabled by default in almost all cases\. 

With Application Auto Scaling, there are two primary ways to enable automatic scaling: using the service console of the resources that you want to scale, and accessing the Application Auto Scaling API programmatically through the AWS Command Line Interface \(AWS CLI\) or using an AWS SDK\. 

If you are just getting started with Application Auto Scaling or are exploring the AWS Management Console, we recommend that you refer to the following documentation for sample configurations and to learn more about how the service integrates with Application Auto Scaling\. If a service doesn't include documentation for configuring scaling in the console, you can use the AWS CLI or AWS SDKs to configure scaling\.
+ AppStream 2\.0 fleets — [Fleet Auto Scaling for AppStream 2\.0](https://docs.aws.amazon.com/appstream2/latest/developerguide/autoscaling.html) in the *Amazon AppStream 2\.0 Administration Guide*
+ Aurora DB clusters — [Using Amazon Aurora Auto Scaling with Aurora replicas](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Integrating.AutoScaling.html) in the *Amazon RDS User Guide*
+ Amazon Comprehend document classification and entity recognizer endpoints — [Auto scaling with endpoints](https://docs.aws.amazon.com/comprehend/latest/dg/comprehend-autoscaling.html) in the *Amazon Comprehend Developer Guide*
+ DynamoDB tables and global secondary indexes — [Managing throughput capacity with DynamoDB Auto Scaling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html) in the *Amazon DynamoDB Developer Guide*
+ Amazon EMR clusters — [Using automatic scaling with a custom policy for instance groups](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-automatic-scaling.html) in the *Amazon EMR Management Guide*
+ ECS services — [Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) in the *Amazon Elastic Container Service Developer Guide*
+ Amazon Keyspaces tables — [Managing Amazon Keyspaces throughput capacity with Application Auto Scaling](https://docs.aws.amazon.com/keyspaces/latest/devguide/autoscaling.html) in the *Amazon Keyspaces \(for Apache Cassandra\) Developer Guide*
+ Lambda function provisioned concurrency — [Managing concurrency for a Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) in the *AWS Lambda Developer Guide*
+ Amazon MSK cluster storage — [Auto\-expanding storage for an Amazon MSK cluster](https://docs.aws.amazon.com/msk/latest/developerguide/msk-autoexpand.html) in the *Amazon Managed Streaming for Apache Kafka Developer Guide*
+ SageMaker endpoint variants — [Automatically scale Amazon SageMaker models](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html) in the *Amazon SageMaker Developer Guide*
+ Spot Fleet requests — [Automatic scaling for Spot Fleet](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-automatic-scaling.html) in the *Amazon EC2 User Guide*
+ Custom resources — [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource)

**Using Application Auto Scaling with the AWS CLI**  
This user guide shows how to work with the Application Auto Scaling API using the AWS CLI\. For introductory exercises, see [Getting started using the AWS CLI](get-started-exercise.md)\. In this tutorial, we show you how to use the AWS CLI to access Application Auto Scaling as a first\-time user\. You learn the basic steps to configure scaling so your DynamoDB table scales at scheduled times\. We chose to use a DynamoDB table as the basis for the tutorial because DynamoDB is available in most Regions, but you can use any supported service that you are comfortable with\.