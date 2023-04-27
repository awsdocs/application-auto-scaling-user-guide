# Custom resources and Application Auto Scaling<a name="services-that-can-integrate-custom"></a>

You can scale custom resources using target tracking scaling policies, step scaling policies, and scheduled scaling\. 

Use the following information to help you integrate custom resources with Application Auto Scaling\. 

If you are just getting started with scaling custom resources, you can view our [GitHub repository](https://github.com/aws/aws-auto-scaling-custom-resource), which provides details about how custom resources integrate with Application Auto Scaling\.

## Service\-linked role created for custom resources<a name="integrate-service-linked-role-custom"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering custom resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_CustomResource`

## Service principal used by the service\-linked role<a name="integrate-service-principal-custom"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `custom-resource.application-autoscaling.amazonaws.com`

## Registering custom resources as scalable targets with Application Auto Scaling<a name="integrate-register-custom"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for a custom resource\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

To configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for a custom resource\. The following example registers a custom resource as a scalable target, with a minimum desired count of one capacity unit and a maximum desired count of 10 capacity units\. The `custom-resource-id.txt` file contains a string that identifies the resource ID, which represents the path to the custom resource through your Amazon API Gateway endpoint\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace custom-resource \
    --scalable-dimension custom-resource:ResourceType:Property \
    --resource-id file://~/custom-resource-id.txt \
    --min-capacity 1 \
    --max-capacity 10
  ```

  Contents of `custom-resource-id.txt`:

  ```
  https://example.execute-api.us-west-2.amazonaws.com/prod/scalableTargetDimensions/1-23456789
  ```

  If successful, this command returns the ARN of the scalable target\.

  ```
  {
      "ScalableTargetARN": "arn:aws:application-autoscaling:region:account-id:scalable-target/1234abcd56ab78cd901ef1234567890ab123"
  }
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 