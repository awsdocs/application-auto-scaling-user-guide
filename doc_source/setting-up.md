# Setting up<a name="setting-up"></a>

Complete the tasks in this section to set up Application Auto Scaling for the first time:

**Topics**
+ [Sign up to AWS](#getting-started-prereqs-signup)
+ [Set up the AWS CLI](setup-awscli.md)
+ [Use AWS CloudShell to work with Application Auto Scaling from the command line](use-awscli-with-cloudshell.md)

## Sign up to AWS<a name="getting-started-prereqs-signup"></a>

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

**Using Application Auto Scaling in AWS Regions**  
Application Auto Scaling is available in multiple AWS Regions\. A global AWS account allows you to work with resources in most Regions\. When using Application Auto Scaling with resources in the China Regions, keep in mind that you must have a separate Amazon Web Services \(China\) account\. In addition, there are some differences in how Application Auto Scaling is implemented\. For more information on using Application Auto Scaling in the China Regions, see [Application Auto Scaling in China](https://docs.amazonaws.cn/en_us/aws/latest/userguide/application-auto-scaling.html)\.

After setting up your AWS account, continue to the next topic: [Set up the AWS CLI](setup-awscli.md)\.