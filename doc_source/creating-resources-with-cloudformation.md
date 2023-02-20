# Creating Application Auto Scaling resources with AWS CloudFormation<a name="creating-resources-with-cloudformation"></a>

Application Auto Scaling is integrated with AWS CloudFormation, a service that helps you to model and set up your AWS resources so that you can spend less time creating and managing your resources and infrastructure\. You create a template that describes all the AWS resources that you want, and AWS CloudFormation provisions and configures those resources for you\. 

When you use AWS CloudFormation, you can reuse your template to set up your Application Auto Scaling resources consistently and repeatedly\. Describe your resources once, and then provision the same resources over and over in multiple AWS accounts and Regions\. 

## Application Auto Scaling and AWS CloudFormation templates<a name="working-with-templates"></a>

To provision and configure resources for Application Auto Scaling and related services, you must understand [AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)\. Templates are formatted text files in JSON or YAML\. These templates describe the resources that you want to provision in your AWS CloudFormation stacks\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer to help you get started with AWS CloudFormation templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\.

When you create a stack template for Application Auto Scaling resources, you must provide the following:
+ A namespace for the target service \(for example, **`appstream`**\)\. See the [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) reference to obtain service namespaces\. 
+ A scalable dimension associated with the target resource \(for example, **`appstream:fleet:DesiredCapacity`**\)\. See the [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) reference to obtain scalable dimensions\. 
+ A resource ID for the target resource \(for example, **`fleet/sample-fleet`**\)\. See the [AWS::ApplicationAutoScaling::ScalableTarget](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-applicationautoscaling-scalabletarget.html) reference for information about the syntax and examples of specific resource IDs\.
+ A service\-linked role for the target resource \(for example, **`arn:aws:iam::012345678910:role/aws-service-role/appstream.application-autoscaling.amazonaws.com/AWSServiceRoleForApplicationAutoScaling_AppStreamFleet`**\)\. See the [Service\-linked role ARN reference](application-auto-scaling-service-linked-roles.md#specify-service-linked-role) table to obtain role ARNs\. 

To learn more about Application Auto Scaling resources, see the [Application Auto Scaling](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_ApplicationAutoScaling.html) reference in the *AWS CloudFormation User Guide*\.

## Example template snippets<a name="example-templates"></a>

We provide a few JSON and YAML template snippets that you can use to understand how to declare various scaling policies and scheduled actions in your stack templates\. For more information, see the [Application Auto Scaling template examples](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-autoscaling.html#scenario-app-as-template-examples) section of the *AWS CloudFormation User Guide*\. For more examples, see the examples sections in the [Application Auto Scaling](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_ApplicationAutoScaling.html) reference in the *AWS CloudFormation User Guide*\.

## Learn more about AWS CloudFormation<a name="learn-more-cloudformation"></a>

To learn more about AWS CloudFormation, see the following resources:
+ [AWS CloudFormation](http://aws.amazon.com/cloudformation/)
+ [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation API Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/Welcome.html)
+ [AWS CloudFormation Command Line Interface User Guide](https://docs.aws.amazon.com/cloudformation-cli/latest/userguide/what-is-cloudformation-cli.html)