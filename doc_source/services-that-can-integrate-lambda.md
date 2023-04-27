# AWS Lambda and Application Auto Scaling<a name="services-that-can-integrate-lambda"></a>

You can scale AWS Lambda provisioned concurrency using target tracking scaling policies and scheduled scaling\. 

Use the following information to help you integrate Lambda with Application Auto Scaling\. 

If you are just getting started with scaling Lambda functions, you can view sample configurations and details about using Lambda with Application Auto Scaling in the following documentation:
+ [Managing Lambda provisioned concurrency](https://docs.aws.amazon.com/lambda/latest/dg/provisioned-concurrency.html) in the *AWS Lambda Developer Guide*

## Service\-linked role created for Lambda<a name="integrate-service-linked-role-lambda"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Lambda resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_LambdaConcurrency`

## Service principal used by the service\-linked role<a name="integrate-service-principal-lambda"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `lambda.application-autoscaling.amazonaws.com`

## Registering Lambda functions as scalable targets with Application Auto Scaling<a name="integrate-register-lambda"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for a Lambda function\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

To configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for a Lambda function\. The following example registers the provisioned concurrency for an alias called `BLUE` for a function called `my-function`, with a minimum capacity of 0 and a maximum capacity of 100\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace lambda \
    --scalable-dimension lambda:function:ProvisionedConcurrency \
    --resource-id function:my-function:BLUE \
    --min-capacity 0 \
    --max-capacity 100
  ```

  If successful, this command returns the ARN of the scalable target\.

  ```
  {
      "ScalableTargetARN": "arn:aws:application-autoscaling:region:account-id:scalable-target/1234abcd56ab78cd901ef1234567890ab123"
  }
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 