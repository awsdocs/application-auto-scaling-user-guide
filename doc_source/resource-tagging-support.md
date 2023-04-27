# Tagging support for Application Auto Scaling<a name="resource-tagging-support"></a>

You can use the AWS CLI or an SDK to tag Application Auto Scaling scalable targets\. Scalable targets are the entities that represent the AWS or custom resources that Application Auto Scaling can scale\. 

Each tag is a label consisting of a user\-defined key and value using the Application Auto Scaling API\. Tags can help you configure granular access to specific scalable targets according to your organization's needs\. For more information, see [ABAC with Application Auto Scaling](security_iam_service-with-iam.md#security_iam_service-with-iam-tags)\.

You can add tags to new scalable targets when you register them, or you can add them to existing scalable targets\.

The commonly used commands for managing tags include: 
+ [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) to tag new scalable targets when you register them\. 
+ [tag\-resource](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/tag-resource.html) to add tags to an existing scalable target\.
+  [list\-tags\-for\-resource](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/list-tags-for-resource.html) to return the tags on a scalable target\.
+ [untag\-resource](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/untag-resource.html) to delete a tag\. 

## Tagging example<a name="tagging-example"></a>

Use the following [register\-scalable\-target](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command with the `--tags` option\. This example tags a scalable target with two tags: a tag key named **environment** with the tag value of **production**, and a tag key named **iscontainerbased** with the tag value of **true**\.

Replace the sample values for `--min-capacity` and `--max-capacity` and sample text for `--service-namespace` with the namespace of the AWS service you're using with Application Auto Scaling, `--scalable-dimension` with the scalable dimension associated with the resource you're registering, and `--resource-id` with an identifier for the resource\. For more information and examples for each service, see the topics in [AWS services that you can use with Application Auto Scaling](integrated-services-list.md)\.

```
aws application-autoscaling register-scalable-target \
  --service-namespace namespace \
  --scalable-dimension dimension \
  --resource-id identifier \
  --min-capacity 1 --max-capacity 10 \
  --tags environment=production,iscontainerbased=true
```

If successful, this command returns the ARN of the scalable target\.

```
{
    "ScalableTargetARN": "arn:aws:application-autoscaling:region:account-id:scalable-target/1234abcd56ab78cd901ef1234567890ab123"
}
```

**Note**  
If this command throws an error, make sure that you have updated the AWS CLI locally to the latest version\.

## Tags for security<a name="tag-security"></a>

Use tags to verify that the requester \(such as an IAM user or role\) has permissions to perform certain actions\. Provide tag information in the condition element of an IAM policy by using one or more of the following condition keys:
+ Use `aws:ResourceTag/tag-key: tag-value` to allow \(or deny\) user actions on scalable targets with specific tags\. 
+ Use `aws:RequestTag/tag-key: tag-value` to require that a specific tag be present \(or not present\) in a request\. 
+ Use `aws:TagKeys [tag-key, ...]` to require that specific tag keys be present \(or not present\) in a request\. 

For example, the following IAM policy grants permissions to use the `DeregisterScalableTarget`, `DeleteScalingPolicy`, and `DeleteScheduledAction` actions\. However, it also denies the actions if the scalable target being acted upon has the tag `environment`=`production`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [        
                "application-autoscaling:DeregisterScalableTarget",
                "application-autoscaling:DeleteScalingPolicy",
                "application-autoscaling:DeleteScheduledAction"
            ],
            "Resource": "*"
            }
        },
        {
            "Effect": "Deny",
            "Action": [        
                "application-autoscaling:DeregisterScalableTarget",
                "application-autoscaling:DeleteScalingPolicy",
                "application-autoscaling:DeleteScheduledAction"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {"aws:ResourceTag/environment": "production"}
            }
        }
    ]
}
```

## Control access to tags<a name="tag-permissions"></a>

Use tags to verify that the requester \(such as an IAM user or role\) has permissions to add, modify, or delete tags for scalable targets\. 

For example, you could create an IAM policy that allows removing only the tag with the `temporary` key from scalable targets\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "application-autoscaling:UntagResource",
            "Resource": "*",
            "Condition": {
                "ForAllValues:StringEquals": { "aws:TagKeys": ["temporary"] }
            }
        }
    ]
}
```