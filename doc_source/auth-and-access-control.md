# Identity and Access Management for Application Auto Scaling<a name="auth-and-access-control"></a>



AWS Identity and Access Management \(IAM\) is an Amazon Web Services \(AWS\) service that helps an administrator securely control access to AWS resources\. IAM administrators control who can be *authenticated* \(signed in\) and *authorized* \(have permissions\) to use AWS resources\. IAM is an AWS service that you can use with no additional charge\. 

To use Application Auto Scaling, you need an AWS account and credentials\. To increase the security of your AWS account, we recommend that you use an *IAM user* to make authenticated requests instead of using your AWS account root user credentials\. You can create an IAM user and grant that user full access\. We refer to these users as administrator users\. You can use the administrator user credentials, instead of AWS account root user credentials, to interact with AWS and perform tasks, such as configuring scaling policies\. For more information, see [AWS account root user credentials vs\. IAM user credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html) in the *AWS General Reference* and [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\. 

After you create an IAM user, you will need to obtain your AWS access keys if you want to access Application Auto Scaling through the Application Auto Scaling API, whether by the Query \(HTTPS\) interface directly or indirectly through an [SDK](https://aws.amazon.com/tools/), the [AWS Command Line Interface](https://aws.amazon.com/cli/), or the [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. AWS access keys consist of an access key ID and a secret access key\. For more information about getting your AWS access keys, see [AWS security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *AWS General Reference*\.

To get started quickly using Application Auto Scaling to set up automatic scaling, go through the steps in [Setting up](setting-up.md)\. As you go through the example tutorial, you create an IAM user and AWS access keys for the specified user\.

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