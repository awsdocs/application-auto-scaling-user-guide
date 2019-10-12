# Application Auto Scaling Limits<a name="application-auto-scaling-limits"></a>

Your AWS account has the following limits related to Application Auto Scaling\. To request a limit increase, use the [Application Auto Scaling Limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-application-auto-scaling)\. 


**Default Limits Per Region Per Account**  

| Item | Default Limit | Notes | 
| --- | --- | --- | 
| Maximum number of scalable targets per resource type |  Amazon DynamoDB: 3000 All other resource types: 500  | Make sure that you specify the type of resource with your request for a limit increase, for example, Amazon ECS or DynamoDB\. | 
| Maximum number of scaling policies per scalable target  | 50 | This includes both step scaling policies and target tracking policies\. | 
| Maximum number of scheduled actions per scalable target | 200 |  | 
| Maximum number of step adjustments per step scaling policy | 20 |  | 

For information about the service limits for other AWS services, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) in the *Amazon Web Services General Reference*\.