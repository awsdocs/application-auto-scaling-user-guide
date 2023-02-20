# Set up the AWS CLI<a name="setup-awscli"></a>

The AWS Command Line Interface \(AWS CLI\) is a unified developer tool for managing AWS services, including Application Auto Scaling\. Follow the steps to download and configure the AWS CLI\.

**To set up the AWS CLI**

1. Download, install, and configure version 1 or 2 of the AWS CLI\. The same Application Auto Scaling functionality is available in version 1 and 2\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*: 

   **AWS CLI version 1**
   +  [Installing, updating, and uninstalling the AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-chap-install.html) 
   +  [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/v1/userguide/cli-chap-configure.html) 

   **AWS CLI version 2**
   +  [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) 
   +  [Quick setup](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html) 
**Note**  
For CLI access, you need an access key ID and a secret access key\. Use temporary credentials instead of long\-term access keys when possible\. Temporary credentials include an access key ID, a secret access key, and a security token that indicates when the credentials expire\. To increase the security of your AWS account, we strongly recommend that you do not use the access credentials associated with your AWS account root user\. For more information, see [Programmatic access](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) in the *AWS General Reference* and [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

1. To confirm that the AWS CLI profile is configured correctly, run the following command in a command window\.

   ```
   aws configure
   ```

   If your profile has been configured correctly, you should see output similar to the following\.

   ```
   AWS Access Key ID [****************52FQ]: 
   AWS Secret Access Key [****************xgyZ]: 
   Default region name [us-east-1]: 
   Default output format [json]:
   ```

1. Run the following command to verify that the Application Auto Scaling commands for the AWS CLI are installed\.

   ```
   aws application-autoscaling help
   ```