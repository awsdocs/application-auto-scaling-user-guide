# Amazon SageMaker and Application Auto Scaling<a name="services-that-can-integrate-sagemaker"></a>

You can scale SageMaker endpoint variants using target tracking scaling policies, step scaling policies, and scheduled scaling\. 

Use the following information to help you integrate SageMaker with Application Auto Scaling\. 

If you are just getting started with scaling SageMaker endpoint variants, you can view sample configurations and details about using SageMaker with Application Auto Scaling in the following documentation:
+ [Automatically scale Amazon SageMaker models](https://docs.aws.amazon.com/sagemaker/latest/dg/endpoint-auto-scaling.html) in the *Amazon SageMaker Developer Guide*

## Service\-linked role created for SageMaker<a name="integrate-service-linked-role-sagemaker"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering SageMaker resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_SageMakerEndpoint`

## Service principal used by the service\-linked role<a name="integrate-service-principal-sagemaker"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `sagemaker.application-autoscaling.amazonaws.com`

## Registering SageMaker endpoint variants as scalable targets with Application Auto Scaling<a name="integrate-register-sagemaker"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for a SageMaker model \(variant\)\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the SageMaker console, then SageMaker automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an SageMaker endpoint variant\. The following example registers the desired EC2 instance count for a product variant called `my-variant`, running on the `my-endpoint` endpoint, with a minimum capacity of one instance and a maximum capacity of eight instances\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace sagemaker \
    --scalable-dimension sagemaker:variant:DesiredInstanceCount \
    --resource-id endpoint/my-endpoint/variant/my-variant \
    --min-capacity 1 \
    --max-capacity 8
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 