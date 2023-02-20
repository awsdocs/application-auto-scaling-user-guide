# Amazon Neptune and Application Auto Scaling<a name="services-that-can-integrate-neptune"></a>

You can scale Neptune clusters using target tracking scaling policies and scheduled scaling\. 

Use the following information to help you integrate Neptune with Application Auto Scaling\. 

If you are just getting started with scaling Neptune clusters, you can view sample configurations and details about using Neptune with Application Auto Scaling in the following documentation:
+ [Auto\-scaling the number of replicas in an Amazon Neptune DB cluster](https://docs.aws.amazon.com/neptune/latest/userguide/manage-console-autoscaling.html) in the *Neptune User Guide*

## Service\-linked role created for Neptune<a name="integrate-service-linked-role-neptune"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Neptune resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_NeptuneCluster`

## Service principal used by the service\-linked role<a name="integrate-service-principal-neptune"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `neptune.application-autoscaling.amazonaws.com`

## Registering Neptune clusters as scalable targets with Application Auto Scaling<a name="integrate-register-neptune"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for a Neptune cluster\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

To configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for a Neptune cluster\. The following example registers the desired capacity of a cluster called `mycluster`, with a minimum capacity of one and a maximum capacity of eight\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace neptune \
    --scalable-dimension neptune:cluster:ReadReplicaCount \
    --resource-id cluster:mycluster \
    --min-capacity 1 \
    --max-capacity 8
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 