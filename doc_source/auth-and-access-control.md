# Identity and Access Management for Application Auto Scaling<a name="auth-and-access-control"></a>



AWS Identity and Access Management \(IAM\) is an AWS service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use Application Auto Scaling resources\. IAM is an AWS service that you can use with no additional charge\.

To use Application Auto Scaling, you need an AWS account and your security credentials for signing into your account\. For more information, see [Setting up](setting-up.md)\.

For complete IAM documentation, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## Access control<a name="access-control"></a>

You can have valid credentials to authenticate your requests, but unless you have permissions you cannot create or access Application Auto Scaling resources\. For example, you must have permissions to create scaling policies, configure scheduled scaling, and so on\. 

The following sections provide details on how an IAM administrator can use IAM to help secure your AWS resources, by controlling who can perform Application Auto Scaling API actions\. 

**Topics**
+ [Access control](#access-control)
+ [How Application Auto Scaling works with IAM](security_iam_service-with-iam.md)
+ [AWS managed policies for Application Auto Scaling](security-iam-awsmanpol.md)
+ [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)
+ [Application Auto Scaling identity\-based policy examples](security_iam_id-based-policy-examples.md)
+ [Troubleshooting access to Application Auto Scaling](security_iam_troubleshoot.md)
+ [Permissions validation for API calls on target resources](security_iam_permission_validation.md)