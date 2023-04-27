# Amazon ECS and Application Auto Scaling<a name="services-that-can-integrate-ecs"></a>

You can scale ECS services using target tracking scaling policies, step scaling policies, and scheduled scaling\. 

Use the following information to help you integrate Amazon ECS with Application Auto Scaling\. 

If you are just getting started with scaling ECS services, you can view sample configurations and details about using Amazon ECS with Application Auto Scaling in the following documentation:
+ [Service Auto Scaling](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-auto-scaling.html) in the *Amazon Elastic Container Service Developer Guide*

## Service\-linked role created for Amazon ECS<a name="integrate-service-linked-role-ecs"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Amazon ECS resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_ECSService`

## Service principal used by the service\-linked role<a name="integrate-service-principal-ecs"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `ecs.application-autoscaling.amazonaws.com`

## Registering ECS services as scalable targets with Application Auto Scaling<a name="integrate-register-ecs"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for an Amazon ECS service\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the Amazon ECS console, then Amazon ECS automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an Amazon ECS service\. The following example registers a scalable target for a service called `sample-app-service`, running on the `default` cluster, with a minimum task count of one task and a maximum task count of 10 tasks\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace ecs \
    --scalable-dimension ecs:service:DesiredCount \
    --resource-id service/default/sample-app-service \
    --min-capacity 1 \
    --max-capacity 10
  ```

  If successful, this command returns the ARN of the scalable target\.

  ```
  {
      "ScalableTargetARN": "arn:aws:application-autoscaling:region:account-id:scalable-target/1234abcd56ab78cd901ef1234567890ab123"
  }
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 