# Getting started with Application Auto Scaling<a name="getting-started"></a>

The following topics provide information to help you start learning and using Application Auto Scaling\. 

## Learn more<a name="getting-started-learn-more"></a>

[AWS services that you can use with Application Auto Scaling](integrated-services-list.md) — This section introduces you to the services that you can scale and helps you set up auto scaling by registering a scalable target\. It also describes each of the IAM service\-linked roles that Application Auto Scaling creates to access resources in the target service\. 

[Target tracking scaling policies for Application Auto Scaling](application-auto-scaling-target-tracking.md) — One of the primary features of Application Auto Scaling is target tracking scaling policies\. Learn how target tracking policies automatically adjust desired capacity to keep utilization at a constant level based on your configured metric and target values\. For example, you can configure target tracking to keep the average CPU utilization for your Spot Fleet at 50 percent\. Application Auto Scaling then launches or terminates EC2 instances as required to keep the aggregated CPU utilization across all servers at 50 percent\.