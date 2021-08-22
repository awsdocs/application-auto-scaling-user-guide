# Amazon AppStream 2\.0 and Application Auto Scaling<a name="services-that-can-integrate-appstream"></a>

You can scale AppStream 2\.0 fleets using target tracking scaling policies, step scaling policies, and scheduled scaling\. 

Use the following information to help you integrate AppStream 2\.0 with Application Auto Scaling\. 

If you are just getting started with scaling AppStream 2\.0 fleets, you can view sample configurations and details about using AppStream 2\.0 with Application Auto Scaling in the following documentation:
+ [Fleet Auto Scaling for AppStream 2\.0](https://docs.aws.amazon.com/appstream2/latest/developerguide/autoscaling.html) in the *Amazon AppStream 2\.0 Administration Guide*

## Service\-linked role created for AppStream 2\.0<a name="integrate-service-linked-role-appstream"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering AppStream 2\.0 resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_AppStreamFleet`

## Service principal used by the service\-linked role<a name="integrate-service-principal-appstream"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `appstream.application-autoscaling.amazonaws.com`

## Registering AppStream 2\.0 fleets as scalable targets with Application Auto Scaling<a name="integrate-register-appstream"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for an AppStream 2\.0 fleet\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the AppStream 2\.0 console, then AppStream 2\.0 automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an AppStream 2\.0 fleet\. The following example registers the desired capacity of a fleet called `sample-fleet`, with a minimum capacity of one fleet instance and a maximum capacity of five fleet instances\.

  ```
  aws application-autoscaling register-scalable-target \
     --service-namespace appstream \
     --scalable-dimension appstream:fleet:DesiredCapacity \
     --resource-id fleet/sample-fleet \
     --min-capacity 1 \
     --max-capacity 5
  ```
+ AWS SDK: 

  Call the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 