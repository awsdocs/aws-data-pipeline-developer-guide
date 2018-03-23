# Controlling Access to Pipelines and Resources<a name="dp-control-access"></a>

Your security credentials identify you to services in AWS and grant you unlimited use of your AWS resources, such as your pipelines\. You can use features of AWS Data Pipeline and AWS Identity and Access Management \(IAM\) to allow AWS Data Pipeline and other users to access your AWS Data Pipeline resources without sharing your security credentials\.

Organizations can share access to pipelines so that the individuals in that organization can develop and maintain them collaboratively\. However, for example, it might be necessary to do the following:
+ Control which IAM users can access specific pipelines
+ Protect a production pipeline from being edited by mistake
+ Allow an auditor to have read\-only access to pipelines, but prevent them from making changes

AWS Data Pipeline integrates with AWS Identity and Access Management \(IAM\), a service that enables you to do the following:
+ Create users and groups under your AWS account
+ Easily share your AWS resources between the users in your AWS account
+ Assign unique security credentials to each user
+ Control each user's access to services and resources
+ Get a single bill for all users in your AWS account

By using IAM with AWS Data Pipeline, you can control whether users in your organization can perform a task using specific API actions and whether they can use specific AWS resources\. You can use IAM policies based on pipeline tags and worker groups to share your pipelines with other users and control the level of access they have\.

**Topics**
+ [IAM Policies for AWS Data Pipeline](dp-iam-resourcebased-access.md)
+ [Example Policies for AWS Data Pipeline](dp-example-tag-policies.md)
+ [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)