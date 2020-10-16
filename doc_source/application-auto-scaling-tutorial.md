# Tutorial: Configuring scaling to increase the availability of your application<a name="application-auto-scaling-tutorial"></a>

In this tutorial, you learn how to scale out and in based on time windows when your application will have a higher than normal workload\. This is helpful when you have an application that can suddenly require a large amount of capacity on a predictable schedule\. You can configure scheduled scaling to add the required capacity to handle the load on your application, and then remove it when it's no longer required\. 

You can use a target tracking scaling policy together with scheduled scaling\. Scheduled scaling automatically initiates changes to your `MinCapacity` and `MaxCapacity` on your behalf, based on a schedule that you specify\. When a scaling policy is active on the resource, it can scale dynamically based on current resource utilization, within the new minimum and maximum capacity range\.

**Note**  
Before you explore this tutorial, we recommend that you first review the following introductory tutorial: [Getting started using the AWS CLI](get-started-exercise.md)\.

**Topics**
+ [Prerequisites](#tutorial-prerequisites)
+ [Step 1: Register your scalable target](#tutorial-register-scalable-target)
+ [Step 2: Create scheduled actions](#tutorial-create-scheduled-actions)
+ [Step 3: Create a target tracking scaling policy](#tutorial-create-target-tracking-policy)
+ [Step 4: Clean up](#tutorial-scaling-clean-up)

## Prerequisites<a name="tutorial-prerequisites"></a>

This tutorial assumes that you have already done the following:
+ You created an AWS account\.
+ You installed and configured the AWS CLI\. 
+ Your account has all of the necessary permissions for registering and deregistering resources as a scalable target with Application Auto Scaling\. It also has all of the necessary permissions for creating scaling policies and scheduled actions\.
+ You have a scalable resource in a non\-production environment available to use for this tutorial\. If you don't already have one, create one before beginning the tutorial\.

Before you begin, consider the following: 

While completing this tutorial, there are two steps in which you set or update your `MinCapacity` and `MaxCapacity` values to 0 to reset the current capacity to 0\. Depending on the resource that you've chosen to use, you might be unable to reset the current capacity to 0 during these steps\. To help you address the issue, a message in the output will indicate that minimum capacity cannot be less than the value specified and will provide the minimum capacity value that the resource can accept\.

To monitor your scaling activities with Application Auto Scaling, you can use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-activities.html) command\. Each scaling event that is triggered by a scaling policy or a scheduled action generates a scaling activity\. 

## Step 1: Register your scalable target<a name="tutorial-register-scalable-target"></a>

Start by registering your resource as a scalable target with Application Auto Scaling\. A scalable target is a resource that Application Auto Scaling can scale out or scale in\.

**To register your scalable target with Application Auto Scaling**
+ Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/register-scalable-target.html) command to register a new scalable target\. Set the `MinCapacity` and `MaxCapacity` values to 0 to reset the current capacity to 0\. 

  **Linux, macOS, or Unix**

  ```
  aws application-autoscaling register-scalable-target \
    --service-namespace service namespace \
    --scalable-dimension scalable dimension of the scalable target \
    --resource-id resource identifier to associate with this scalable target \
    --min-capacity 0 --max-capacity 0
  ```

  **Windows**

  ```
  aws application-autoscaling register-scalable-target --service-namespace service namespace --scalable-dimension scalable dimension of the scalable target --resource-id resource identifier to associate with this scalable target --min-capacity 0 --max-capacity 0
  ```

  This command does not return any output if it is successful\.

## Step 2: Create scheduled actions<a name="tutorial-create-scheduled-actions"></a>

You can use the [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command to create scheduled actions that are configured to meet your business needs\. In this tutorial, we focus on a configuration that turns off scaling outside of working hours\.

**To create a scheduled action that scales out in the morning**

1. To scale out the scalable target, use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\. Include the `--schedule` parameter with a recurring schedule, in UTC, using a cron expression\. 

   On the specified schedule \(every day at 9:00 AM UTC\), Application Auto Scaling updates the `MinCapacity` and `MaxCapacity` values to the desired range of 1\-5 capacity units\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling put-scheduled-action \
     --service-namespace namespace \
     --scalable-dimension dimension \
     --resource-id identifier \
     --scheduled-action-name my-first-scheduled-action \
     --schedule "cron(0 9 * * ? *)" \
     --scalable-target-action MinCapacity=1,MaxCapacity=5
   ```

   **Windows**

   ```
   aws application-autoscaling put-scheduled-action --service-namespace namespace --scalable-dimension dimension --resource-id identifier --scheduled-action-name my-first-scheduled-action --schedule "cron(0 9 * * ? *)" --scalable-target-action MinCapacity=1,MaxCapacity=5
   ```

   This command does not return any output if it is successful\.

1. To confirm that your scheduled action exists, use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) command\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling describe-scheduled-actions \
     --service-namespace namespace \ 
     --query 'ScheduledActions[?ResourceId==`identifier`]'
   ```

   **Windows**

   ```
   aws application-autoscaling describe-scheduled-actions --service-namespace namespace --query "ScheduledActions[?ResourceId==`identifier`]"
   ```

   The following is example output\.

   ```
   [
       {
           "ScheduledActionName": "my-first-scheduled-action",
           "ScheduledActionARN": "arn",
           "Schedule": "cron(0 9 * * ? *)",
           "ScalableTargetAction": {
               "MinCapacity": 1,
               "MaxCapacity": 5
           },
           ...
       }
   ]
   ```

**To create a scheduled action that scales in at night**

1. Repeat the preceding procedure to create another scheduled action that Application Auto Scaling uses to scale in at the end of the day\. 

   On the specified schedule \(every day at 8:00 PM UTC\), Application Auto Scaling updates the target's `MinCapacity` and `MaxCapacity` to 0, as instructed by the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scheduled-action.html) command\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling put-scheduled-action \
     --service-namespace namespace \
     --scalable-dimension dimension \
     --resource-id identifier \
     --scheduled-action-name my-second-scheduled-action \
     --schedule "cron(0 20 * * ? *)" \
     --scalable-target-action MinCapacity=0,MaxCapacity=0
   ```

   **Windows**

   ```
   aws application-autoscaling put-scheduled-action --service-namespace namespace --scalable-dimension dimension --resource-id identifier --scheduled-action-name my-second-scheduled-action --schedule "cron(0 20 * * ? *)" --scalable-target-action MinCapacity=0,MaxCapacity=0
   ```

1. To confirm that your scheduled action exists, use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scheduled-actions.html) command\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling describe-scheduled-actions \
     --service-namespace namespace \ 
     --query 'ScheduledActions[?ResourceId==`identifier`]'
   ```

   **Windows**

   ```
   aws application-autoscaling describe-scheduled-actions --service-namespace namespace --query "ScheduledActions[?ResourceId==`identifier`]"
   ```

   The following is example output\.

   ```
   [
       {
           "ScheduledActionName": "my-first-scheduled-action",
           "ScheduledActionARN": "arn",
           "Schedule": "cron(0 9 * * ? *)",
           "ScalableTargetAction": {
               "MinCapacity": 1,
               "MaxCapacity": 5
           },
           ...
       },
       {
           "ScheduledActionName": "my-second-scheduled-action",
           "ScheduledActionARN": "arn",
           "Schedule": "cron(0 20 * * ? *)",
           "ScalableTargetAction": {
               "MinCapacity": 0,
               "MaxCapacity": 0
           },
           ...
       }
   ]
   ```

## Step 3: Create a target tracking scaling policy<a name="tutorial-create-target-tracking-policy"></a>

Now that you have the basic schedule in place, add a target tracking scaling policy to scale based on target tracking\. 

With target tracking, Application Auto Scaling compares the target value in the policy to the current value of the specified metric\. When they are unequal for a period of time, Application Auto Scaling adds or removes capacity to maintain steady performance\. As the load on your application and the metric value increases, Application Auto Scaling adds capacity as fast as it can without going above `MaxCapacity`\. When Application Auto Scaling removes capacity because the load is minimal, it does so without going below `MinCapacity`\. By adjusting the capacity based on usage, you only pay for what your application needs\. For more information, see [Supporting application availability during high utilization periods](application-auto-scaling-target-tracking.md#target-tracking-high-utilization)\.

If the metric has insufficient data because your application does not have any load, Application Auto Scaling does not add or remove capacity\. The intention of this behavior is to prioritize availability in situations where not enough information is available\.

**To create a target tracking scaling policy**

1. Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/put-scaling-policy.html) command to create the policy\. 

   The metrics that are most frequently used for target tracking are predefined, and you can use them without supplying the full metric specification from CloudWatch\. For more information about the available predefined metrics, see [PredefinedMetricSpecification](https://docs.aws.amazon.com/autoscaling/application/APIReference/API_PredefinedMetricSpecification.html) in the *Application Auto Scaling API Reference*\. 

   Before you run this command, make sure that your predefined metric expects the target value\. For example, to scale out when CPU reaches 50% utilization, specify a target value of 50\.0\. Or, to scale out Lambda provisioned concurrency when usage reaches 70% utilization, specify a target value of 0\.7\. For information about target values for a particular resource, refer to the documentation that is provided by the service about how to configure target tracking\. For more information, see [Getting started with Application Auto Scaling](getting-started.md)\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling put-scaling-policy \
     --service-namespace namespace \
     --scalable-dimension dimension \
     --resource-id identifier \
     --policy-name my-scaling-policy --policy-type TargetTrackingScaling \
     --target-tracking-scaling-policy-configuration '{ "TargetValue": 50.0, "PredefinedMetricSpecification": { "PredefinedMetricType": "predefinedmetric" }}'
   ```

   **Windows**

   ```
   aws application-autoscaling put-scaling-policy --service-namespace namespace --scalable-dimension dimension --resource-id identifier --policy-name my-scaling-policy --policy-type TargetTrackingScaling --target-tracking-scaling-policy-configuration "{ \"TargetValue\": 50.0, \"PredefinedMetricSpecification\": { \"PredefinedMetricType\": \"predefinedmetric\" }}"
   ```

   If successful, this command returns the ARNs and names of the two CloudWatch alarms that were created on your behalf\.

1. To confirm that your scheduled action exists, use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/describe-scaling-policies.html) command\.

   **Linux, macOS, or Unix**

   ```
   aws application-autoscaling describe-scaling-policies --service-namespace namespace \
     --query 'ScalingPolicies[?ResourceId==`identifier`]'
   ```

   **Windows**

   ```
   aws application-autoscaling describe-scaling-policies --service-namespace namespace --query "ScalingPolicies[?ResourceId==`identifier`]"
   ```

   The following is example output\.

   ```
   [
       {
           "PolicyARN": "arn",
           "TargetTrackingScalingPolicyConfiguration": {
               "PredefinedMetricSpecification": {
                   "PredefinedMetricType": "predefinedmetric"
               },
               "TargetValue": 50.0
           },
           "PolicyName": "my-scaling-policy",
           "PolicyType": "TargetTrackingScaling",
           "Alarms": [],
           ...
       }
   ]
   ```

## Step 4: Clean up<a name="tutorial-scaling-clean-up"></a>

To prevent your account from accruing charges for resources created while actively scaling, you can clean up the associated scaling configuration as follows\. 

Deleting the scaling configuration does not delete your scalable resource\. It also does not return it to its original capacity\. You can use the console of the service where you created the scalable resource to delete it or adjust its capacity\.

**To delete the scheduled actions**  
The following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scheduled-action.html) command deletes a specified scheduled action\. You can skip this step if you want to keep the scheduled actions that you created\.

**Linux, macOS, or Unix**

```
aws application-autoscaling delete-scheduled-action \
  --service-namespace namespace \
  --scalable-dimension dimension \
  --resource-id identifier \
  --scheduled-action-name my-second-scheduled-action
```

**Windows**

```
aws application-autoscaling delete-scheduled-action --service-namespace namespace --scalable-dimension dimension --resource-id identifier --scheduled-action-name my-second-scheduled-action
```

**To delete the scaling policy**  
The following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/delete-scaling-policy.html) command deletes a specified target tracking scaling policy\. You can skip this step if you want to keep the scaling policy that you created\.

**Linux, macOS, or Unix**

```
aws application-autoscaling delete-scaling-policy \
  --service-namespace namespace \
  --scalable-dimension dimension \
  --resource-id identifier \
  --policy-name my-scaling-policy
```

**Windows**

```
aws application-autoscaling delete-scaling-policy --service-namespace namespace --scalable-dimension dimension --resource-id identifier --policy-name my-scaling-policy
```

**To deregister the scalable target**  
Use the following [https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/deregister-scalable-target.html](https://docs.aws.amazon.com/cli/latest/reference/application-autoscaling/deregister-scalable-target.html) command to deregister the scalable target\. If you have any scaling policies that you created or any scheduled actions that have not yet been deleted, they are deleted by this command\. You can skip this step if you want to keep the scalable target registered for future use\.

**Linux, macOS, or Unix**

```
aws application-autoscaling deregister-scalable-target \
  --service-namespace namespace \
  --scalable-dimension dimension \
  --resource-id identifier
```

**Windows**

```
aws application-autoscaling deregister-scalable-target --service-namespace namespace --scalable-dimension dimension --resource-id identifier
```