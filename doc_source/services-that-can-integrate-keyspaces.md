# Amazon Keyspaces \(for Apache Cassandra\) and Application Auto Scaling<a name="services-that-can-integrate-keyspaces"></a>

You can scale Amazon Keyspaces tables using target tracking scaling policies and scheduled scaling\. 

Use the following information to help you integrate Amazon Keyspaces with Application Auto Scaling\. 

If you are just getting started with scaling Amazon Keyspaces tables, you can view sample configurations and details about using Amazon Keyspaces with Application Auto Scaling in the following documentation:
+ [Managing Amazon Keyspaces throughput capacity with Application Auto Scaling](https://docs.aws.amazon.com/keyspaces/latest/devguide/autoscaling.html) in the *Amazon Keyspaces \(for Apache Cassandra\) Developer Guide*

## Service\-linked role created for Amazon Keyspaces<a name="integrate-service-linked-role-keyspaces"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering Amazon Keyspaces resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_CassandraTable`

## Service principal used by the service\-linked role<a name="integrate-service-principal-keyspaces"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `cassandra.application-autoscaling.amazonaws.com`

## Registering Amazon Keyspaces tables as scalable targets with Application Auto Scaling<a name="integrate-register-keyspaces"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for an Amazon Keyspaces table\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the Amazon Keyspaces console, then Amazon Keyspaces automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for an Amazon Keyspaces table\. The following example registers the provisioned write capacity of a table called `mytable`, with a minimum capacity of five write capacity units and a maximum capacity of 10 write capacity units\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace cassandra \
    --scalable-dimension cassandra:table:WriteCapacityUnits \
    --resource-id keyspace/mykeyspace/table/mytable \
    --min-capacity 5 \
    --max-capacity 10
  ```

  The following example registers the provisioned read capacity of a table called `mytable`, with a minimum capacity of five read capacity units and a maximum capacity of 10 read capacity units\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace cassandra \
    --scalable-dimension cassandra:table:ReadCapacityUnits \
    --resource-id keyspace/mykeyspace/table/mytable \
    --min-capacity 5 \
    --max-capacity 10
  ```
+ AWS SDK: 

  Call the [https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 