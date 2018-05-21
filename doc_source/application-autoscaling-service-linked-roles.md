# Service\-Linked Roles for Application Auto Scaling<a name="application-autoscaling-service-linked-roles"></a>

Application Auto Scaling uses service\-linked roles for the permissions that it requires to call other AWS services on your behalf\. For more information, see [Using Service\-Linked Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

## Permissions Granted by the Service\-Linked Roles<a name="service-linked-role-permissions"></a>

Application Auto Scaling uses the following service\-linked roles to make the specified calls on your behalf\. There is one service\-linked role per type of scalable resource\. Each service\-linked role trusts the specified service principal to assume it\.

**AWSServiceRoleForApplicationAutoScaling\_AppStreamFleet**

Actions:
+ `appstream:DescribeFleets`
+ `appstream:UpdateFleet`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `appstream.application-autoscaling.amazonaws.com`

**AWSServiceRoleForApplicationAutoScaling\_DynamoDBTable**

Actions:
+ `dynamodb:DescribeTable`
+ `dynamodb:UpdateTable`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `dynamodb.application-autoscaling.amazonaws.com`

**AWSServiceRoleForApplicationAutoScaling\_EC2SpotFleetRequest**

Actions:
+ `ec2:DescribeSpotFleetRequests`
+ `ec2:ModifySpotFleetRequest`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `ec2.application-autoscaling.amazonaws.com`

**AWSServiceRoleForApplicationAutoScaling\_ECSService**

Actions:
+ `ecs:DescribeServices`
+ `ecs:UpdateService`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `ecs.application-autoscaling.amazonaws.com`

**AWSServiceRoleForApplicationAutoScaling\_RDSCluster**

Actions:
+ `rds:AddTagsToResource`
+ `rds:CreateDBInstance`
+ `rds:DeleteDBInstance`
+ `rds:DescribeDBClusters`
+ `rds:DescribeDBInstance`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `rds.application-autoscaling.amazonaws.com`

**AWSServiceRoleForApplicationAutoScaling\_SageMakerEndpoint**

Actions:
+ `sagemaker:DescribeEndpoint`
+ `sagemaker:DescribeEndpointConfig`
+ `sagemaker:UpdateEndpointWeightsAndCapacities`
+ `cloudwatch:DeleteAlarms`
+ `cloudwatch:DescribeAlarms`
+ `cloudwatch:PutMetricAlarm`

Service principal: `sagemaker.application-autoscaling.amazonaws.com`

## Create Service\-Linked Roles<a name="create-service-linked-role"></a>

You don't need to manually create the service\-linked roles for Application Auto Scaling\. Application Auto Scaling creates the appropriate service\-linked role for you when you call `RegisterScalableTarget` or `PutScalingPolicy`\. For example, if you set up automatic scaling for an ECS service, Application Auto Scaling creates the `AWSServiceRoleForApplicationAutoScaling_ECSService` role\.

## Edit the Service\-Linked Roles<a name="edit-service-linked-role"></a>

You can edit the description of the service\-linked roles created for Application Auto Scaling using IAM\. For more information, see [Editing a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Delete the Service\-Linked Roles<a name="delete-service-linked-role"></a>

If you no longer need to use Application Auto Scaling with one type of scalable resource, we recommend that you delete the corresponding service\-linked role\.

You can delete a service\-linked role only after first deleting the related scalable resources\. This protects your resources because you can't inadvertently remove permission to access them\. For more information, see the documentation for the scalable resource\. For example, to delete an ECS service, see [Deleting a Service](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/delete-service.html) in the *Amazon Elastic Container Service Developer Guide*\.

You can use the IAM console, the IAM CLI, or the IAM API to delete service\-linked roles\. For more information, see [Deleting a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

After you delete a service\-linked role, Application Auto Scaling will create the role again when you call `RegisterScalableTarget` or `PutScalingPolicy`\.