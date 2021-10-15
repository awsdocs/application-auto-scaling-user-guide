# Amazon Aurora and Application Auto Scaling<a name="services-that-can-integrate-aurora"></a>

You can scale Aurora DB clusters using target tracking scaling policies, step scaling policies, and scheduled scaling\. 

Use the following information to help you integrate Aurora with Application Auto Scaling\. 

If you are just getting started with scaling Aurora DB clusters, you can view sample configurations and details about using Aurora with Application Auto Scaling in the following documentation:
+ [Using Amazon Aurora Auto Scaling with Aurora replicas](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Integrating.AutoScaling.html) in the *Amazon RDS User Guide*

## Service\-linked role created for Aurora<a name="integrate-service-linked-role-aurora"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Aurora resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_RDSCluster`

## Service principal used by the service\-linked role<a name="integrate-service-principal-aurora"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `rds.application-autoscaling.amazonaws.com`

## Registering Aurora DB clusters as scalable targets with Application Auto Scaling<a name="integrate-register-aurora"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for an Aurora cluster\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the Aurora console, then Aurora automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an Aurora cluster\. The following example registers the count of Aurora Replicas in a cluster called `my-db-cluster`, with a minimum capacity of one Aurora Replica and a maximum capacity of eight Aurora Replicas\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace rds \
    --scalable-dimension rds:cluster:ReadReplicaCount \
    --resource-id cluster:my-db-cluster \
    --min-capacity 1 \
    --max-capacity 8
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 