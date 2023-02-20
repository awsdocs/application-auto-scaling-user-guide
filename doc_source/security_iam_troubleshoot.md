# Troubleshooting access to Application Auto Scaling<a name="security_iam_troubleshoot"></a>

If you encounter `AccessDeniedException` or similar difficulties when working with Application Auto Scaling, consult the information in this section\. 

## I am not authorized to perform an action in Application Auto Scaling<a name="security_iam_troubleshoot-no-permissions"></a>

If you receive an `AccessDeniedException` when calling an AWS API operation, it means that the AWS Identity and Access Management \(IAM\) credentials that you are using do not have the required permissions to make that call\. 

The following example error occurs when the `mateojackson` user tries to view details about a scalable target, but does not have `application-autoscaling:DescribeScalableTargets` permission\.

```
An error occurred (AccessDeniedException) when calling the DescribeScalableTargets operation: User: arn:aws:iam::123456789012:user/mateojackson is not authorized to perform: application-autoscaling:DescribeScalableTargets
```

If you receive this or similar errors, then you must contact your administrator for assistance\. 

An administrator for your account will need to make sure that you have permissions to access all of the API actions that Application Auto Scaling uses to access resources in the target service and CloudWatch\. There are different permissions required depending on which resources you are working with\. Application Auto Scaling also requires permission to create a service\-linked role the first time that a user configures scaling for a given resource\. 

## I'm an administrator and my IAM policy returned an error or isn't working as expected<a name="security_iam_troubleshoot-validation-errors"></a>

In addition to Application Auto Scaling actions, your IAM policies must grant permissions to call the target service and CloudWatch\. If a user or application doesn't have these additional permissions, their access might be unexpectedly denied\. To write IAM policies for users and applications in your accounts, consult the information in [Application Auto Scaling identity\-based policy examples](security_iam_id-based-policy-examples.md)\. 

For information about how validation is performed, see [Permissions validation for API calls on target resources](security_iam_permission_validation.md)\.

Note that some permission issues can also be due to an issue with creating the service\-linked roles used by Application Auto Scaling\. For information about creating these service\-linked roles, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.