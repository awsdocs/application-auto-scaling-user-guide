# Amazon DynamoDB and Application Auto Scaling<a name="services-that-can-integrate-dynamodb"></a>

You can scale DynamoDB tables and global secondary indexes using target tracking scaling policies and scheduled scaling\. 

Use the following information to help you integrate DynamoDB with Application Auto Scaling\. 

If you are just getting started with scaling DynamoDB tables and global secondary indexes, you can view sample configurations and details about using DynamoDB with Application Auto Scaling in the following documentation:
+ [Managing throughput capacity with DynamoDB Auto Scaling](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html) in the *Amazon DynamoDB Developer Guide*

**Tip**  
We also provide a tutorial for scheduled scaling in [Getting started using the AWS CLI](get-started-exercise.md)\. In this tutorial, you learn the basic steps to configure scaling so your DynamoDB table scales at scheduled times\.

## Service\-linked role created for DynamoDB<a name="integrate-service-linked-role-dynamodb"></a>

The following [service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) is automatically created in your AWS account when registering DynamoDB resources as scalable targets with Application Auto Scaling\. This role allows Application Auto Scaling to perform supported operations within your account\. For more information, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.
+ `AWSServiceRoleForApplicationAutoScaling_DynamoDBTable`

## Service principal used by the service\-linked role<a name="integrate-service-principal-dynamodb"></a>

The service\-linked role in the previous section can be assumed only by the service principal authorized by the trust relationships defined for the role\. The service\-linked role used by Application Auto Scaling grants access to the following service principal: 
+ `dynamodb.application-autoscaling.amazonaws.com`

## Registering DynamoDB resources as scalable targets with Application Auto Scaling<a name="integrate-register-dynamodb"></a>

Application Auto Scaling requires a scalable target before you can create scaling policies or scheduled actions for a DynamoDB table or global secondary index\. A scalable target is a resource that Application Auto Scaling can scale out and scale in\. Scalable targets are uniquely identified by the combination of resource ID, scalable dimension, and namespace\. 

If you configure auto scaling using the DynamoDB console, then DynamoDB automatically registers a scalable target for you\. 

If you want to configure auto scaling using the AWS CLI or one of the AWS SDKs, you can use the following options:
+ AWS CLI: 

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for a table\. The following example registers the provisioned write capacity of a table called `my-table`, with a minimum capacity of five write capacity units and a maximum capacity of 10 write capacity units\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace dynamodb \
    --scalable-dimension dynamodb:table:WriteCapacityUnits \
    --resource-id table/my-table \
    --min-capacity 5 \
    --max-capacity 10
  ```

  Call the [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command for a global secondary index\. The following example registers the provisioned write capacity of a global secondary index called `my-table-index`, with a minimum capacity of five write capacity units and a maximum capacity of 10 write capacity units\.

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace dynamodb \
    --scalable-dimension dynamodb:index:WriteCapacityUnits \
    --resource-id table/my-table/index/my-table-index \
    --min-capacity 5 \
    --max-capacity 10
  ```
+ AWS SDK:

  Call the [RegisterScalableTarget](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_RegisterScalableTarget.html) operation and provide `ResourceId`, `ScalableDimension`, `ServiceNamespace`, `MinCapacity`, and `MaxCapacity` as parameters\. 