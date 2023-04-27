# Amazon Managed Streaming for Apache Kafka \(MSK\) and Application Auto Scaling<a name="services-that-can-integrate-msk"></a>

You can scale out Amazon MSK cluster storage using target tracking scaling policies\. Scale in by the target tracking policy is disabled\. 

Use the following information to help you integrate Amazon MSK with Application Auto Scaling\. 

If you are just getting started with scaling Amazon MSK cluster storage, you can view details about using Amazon MSK with Application Auto Scaling in the following documentation:
+ [Auto\-expanding storage for an Amazon MSK cluster](https://docs.aws.amazon.com/msk/latest/developerguide/msk-autoexpand.html) in the *Amazon Managed Streaming for Apache Kafka Developer Guide*

## Service\-linked role created for Amazon MSK<a name="integrate-service-linked-role-msk"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Amazon MSK resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_KafkaCluster`

## Service principal used by the service\-linked role<a name="integrate-service-principal-msk"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `kafka.application-autoscaling.amazonaws.com`

## Registering Amazon MSK cluster storage as scalable targets with Application Auto Scaling<a name="integrate-register-msk"></a>

Application Auto Scaling requires a scalable target before you can create a scaling policy for the storage volume size per broker of an Amazon MSK cluster\. A scalable target is a resource that Application Auto Scaling can scale\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the Amazon MSK console, then Amazon MSK automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an Amazon MSK cluster\. The following example registers the storage volume size per broker of an Amazon MSK cluster, with a minimum capacity of 100 GiB and a maximum capacity of 800 GiB\. 

  ```
  aws application-autoscaling register-scalable-target \
     --service-namespace kafka \
     --scalable-dimension kafka:broker-storage:VolumeSize \
     --resource-id arn:aws:kafka:us-east-1:123456789012:cluster/demo-cluster-1/6357e0b2-0e6a-4b86-a0b4-70df934c2e31-5 \
     --min-capacity 100 \
     --max-capacity 800
  ```

  If successful, this command returns the ARN of the scalable target\.

  ```
  {
      "ScalableTargetARN": "arn:aws:application-autoscaling:region:account-id:scalable-target/1234abcd56ab78cd901ef1234567890ab123"
  }
  ```
+ AWS SDK: 

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 

**Note**  
When an Amazon MSK cluster is the scalable target, scale in is disabled and cannot be enabled\. 