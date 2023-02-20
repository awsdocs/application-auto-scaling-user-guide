# How Application Auto Scaling works with IAM<a name="security_iam_service-with-iam"></a>

**Note**  
In December 2017, there was an update for Application Auto Scaling, enabling several service\-linked roles for Application Auto Scaling integrated services\. Specific IAM permissions *and* an Application Auto Scaling service\-linked role \(or a service role for Amazon EMR auto scaling\) are required so that users can configure scaling\. 

Before you use IAM to manage access to Application Auto Scaling, learn what IAM features are available to use with Application Auto Scaling\.


**IAM features you can use with Application Auto Scaling**  

| IAM feature | Application Auto Scaling support | 
| --- | --- | 
|  [Identity\-based policies](#security_iam_service-with-iam-id-based-policies)  |    Yes  | 
|  [Policy actions](#security_iam_service-with-iam-id-based-policies-actions)  |    Yes  | 
|  [Policy resources](#security_iam_service-with-iam-id-based-policies-resources)  |    No   | 
|  [Policy condition keys \(service\-specific\)](#security_iam_service-with-iam-id-based-policies-conditionkeys)  |    No   | 
|  [Resource\-based policies](#security_iam_service-with-iam-resource-based-policies)  |    No   | 
|  [ACLs](#security_iam_service-with-iam-acls)  |    No   | 
|  [Authorization based on tags](#security_iam_service-with-iam-tags)  |    No   | 
|  [Temporary credentials](#security_iam_service-with-iam-roles-tempcreds)  |    Yes  | 
|  [Service roles](#security_iam_service-with-iam-roles-service)  |    Yes  | 
|  [Service\-linked roles](#security_iam_service-with-iam-roles-service-linked)  |    Yes  | 

To get a high\-level view of how Application Auto Scaling and other AWS services work with most IAM features, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

## Application Auto Scaling identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports identity\-based policies  |    Yes  | 

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, group of users, or role\. These policies control what actions users and roles can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. You can't specify the principal in an identity\-based policy because it applies to the user or role to which it is attached\. To learn about all of the elements that you can use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Identity\-based policy examples for Application Auto Scaling<a name="security_iam_service-with-iam-id-based-policies-examples"></a>

To view examples of Application Auto Scaling identity\-based policies, see [Application Auto Scaling identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>


|  |  | 
| --- |--- |
|  Supports policy actions  |    Yes  | 

In an IAM policy statement, you can specify any API action from any service that supports IAM\. For Application Auto Scaling, use the following prefix with the name of the API action: `application-autoscaling:`\. For example: `application-autoscaling:RegisterScalableTarget`, `application-autoscaling:PutScalingPolicy`, and `application-autoscaling:DeregisterScalableTarget`\.

To specify multiple actions in a single statement, separate them with commas as shown in the following example\.

```
"Action": [
      "application-autoscaling:DescribeScalingPolicies",
      "application-autoscaling:DescribeScalingActivities"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action\.

```
"Action": "application-autoscaling:Describe*"
```

For a complete list of policy actions for Application Auto Scaling, see [Actions defined by AWS Application Auto Scaling](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awsapplicationautoscaling.html#awsapplicationautoscaling-actions-as-permissions) in the *Service Authorization Reference*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>


|  |  | 
| --- |--- |
|  Supports policy resources  |    No   | 

The `Resource` element specifies the object or objects to which the action applies\.

Application Auto Scaling has no service\-defined resources that can be used as the `Resource` element of an IAM policy statement\. Therefore, there are no Amazon Resource Names \(ARNs\) for Application Auto Scaling for you to use in an IAM policy\. To control access to Application Auto Scaling API actions, always use an \* \(asterisk\) as the resource when writing an IAM policy\. 

### Condition keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>


|  |  | 
| --- |--- |
|  Supports service\-specific policy condition keys  |    No   | 

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. For example, you might want a policy to be applied only after a specific date\. To express conditions, use predefined condition keys\.

Application Auto Scaling does not provide any service\-specific condition keys, but it does support using some global condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\. 

The `Condition` element is optional\. 

## Resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports resource\-based policies  |    No   | 

Other AWS services, such as Amazon Simple Storage Service, support resource\-based permissions policies\. For example, you can attach a permissions policy to an S3 bucket to manage access permissions to that bucket\. 

Application Auto Scaling does not support resource\-based policies\.

## Access Control Lists \(ACLs\)<a name="security_iam_service-with-iam-acls"></a>


|  |  | 
| --- |--- |
|  Supports ACLs  |    No   | 

Application Auto Scaling does not support Access Control Lists \(ACLs\)\.

## Authorization based on tags<a name="security_iam_service-with-iam-tags"></a>


|  |  | 
| --- |--- |
|  Supports ABAC \(tags in policies\)  |    No   | 

Application Auto Scaling has no service\-defined resources that can be tagged\. Therefore, it does not support controlling access based on tags\.

## Using temporary credentials with Application Auto Scaling<a name="security_iam_service-with-iam-roles-tempcreds"></a>


|  |  | 
| --- |--- |
|  Supports temporary credentials  |    Yes  | 

Some AWS services don't work when you sign in using temporary credentials\. For additional information, including which AWS services work with temporary credentials, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

You are using temporary credentials if you sign in to the AWS Management Console using any method except a user name and password\. For example, when you access AWS using your company's single sign\-on \(SSO\) link, that process automatically creates temporary credentials\. You also automatically create temporary credentials when you sign in to the console as a user and then switch roles\. For more information about switching roles, see [Switching to a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) in the *IAM User Guide*\.

You can manually create temporary credentials using the AWS CLI or AWS API\. You can then use those temporary credentials to access AWS\. AWS recommends that you dynamically generate temporary credentials instead of using long\-term access keys\. For more information, see [Temporary security credentials in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\.

## Service roles<a name="security_iam_service-with-iam-roles-service"></a>


|  |  | 
| --- |--- |
|  Supports service roles  |    Yes  | 

If your Amazon EMR cluster uses automatic scaling, this feature allows Application Auto Scaling to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. Similar to a service\-linked role, a service role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

Application Auto Scaling supports service roles only for Amazon EMR\. For documentation for the EMR service role, see [Using automatic scaling with a custom policy for instance groups](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-automatic-scaling.html) in the *Amazon EMR Management Guide*\.

**Note**  
With the introduction of service\-linked roles, several legacy service roles are no longer required, for example, for Amazon ECS and Spot Fleet\.

## Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>


|  |  | 
| --- |--- |
|  Supports service\-linked roles  |    Yes  | 

  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your AWS account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 

For information about Application Auto Scaling service\-linked roles, see [Service\-linked roles for Application Auto Scaling](application-auto-scaling-service-linked-roles.md)\.