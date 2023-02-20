# Quotas for Application Auto Scaling<a name="application-auto-scaling-quotas"></a>

Your AWS account has default quotas, formerly referred to as limits, for each AWS service\. Unless otherwise noted, each quota is Region\-specific\. You can request increases for some quotas, and other quotas cannot be increased\.

To view the quotas for Application Auto Scaling, open the [Service Quotas console](https://console.aws.amazon.com/servicequotas/home)\. In the navigation pane, choose **AWS services** and select **Application Auto Scaling**\.

To request a quota increase, see [Requesting a Quota Increase](https://docs.aws.amazon.com/servicequotas/latest/userguide/request-quota-increase.html) in the *Service Quotas User Guide*\. If the quota is not yet available in Service Quotas, use the [Application Auto Scaling limits form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-application-auto-scaling)\. Make sure that you specify the type of resource with your request for an increase, for example, Amazon ECS or DynamoDB\.

Your AWS account has the following quotas related to Application Auto Scaling\.


**Default quotas per Region per account**  

| Item | Default | Adjustable | 
| --- | --- | --- | 
| Maximum number of scalable targets per resource type |  Default quotas vary depending on resource type\.  Up to 5000 Amazon DynamoDB scalable targets, 3000 ECS scalable targets, and 500 scalable targets each for all other resource types\.  | Yes | 
| Maximum number of scaling policies per scalable target  | 50Includes both step scaling policies and target tracking policies\. | No | 
| Maximum number of scheduled actions per scalable target | 200 | No | 
| Maximum number of step adjustments per step scaling policy | 20 | No | 

Keep service quotas in mind as you scale out your workloads\. For example, when you reach the maximum number of capacity units allowed by a service, scaling out will stop\. If demand drops and the current capacity decreases, Application Auto Scaling can scale out again\. To avoid reaching this capacity limit again, you can request an increase\. Each service has its own default quotas for the maximum capacity of the resource\. For information about the default quotas for other AWS services, see [Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/aws-service-information.html) in the *Amazon Web Services General Reference*\. 