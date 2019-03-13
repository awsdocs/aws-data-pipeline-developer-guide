# Setting up for AWS Data Pipeline<a name="dp-get-setup"></a>

Before you use AWS Data Pipeline for the first time, complete the following tasks\.

**Topics**
+ [Sign up for AWS](#dp-sign-up)
+ [Create the Required IAM Roles \(CLI or API Only\)](#dp-iam-roles-new)
+ [Assign a Managed Policy with PassRole to Predefined IAM Roles](#dp-iam-create-user-groups)
+ [Assign an Inline Policy with PassRole to Custom IAM Roles](#dp-iam-inline-policy-passRole)

After you complete these tasks, you can start using AWS Data Pipeline\. For a basic tutorial, see [Getting Started with AWS Data Pipeline](dp-getting-started.md)\.

## Sign up for AWS<a name="dp-sign-up"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including AWS Data Pipeline\. You are charged only for the services that you use\. For more information about AWS Data Pipeline usage rates, see [AWS Data Pipeline](http://aws.amazon.com/datapipeline/)\.

If you have an AWS account already, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
If you previously signed in to the AWS Management Console using AWS account root user credentials, choose **Sign in to a different account**\. If you previously signed in to the console using IAM credentials, choose **Sign\-in using root account credentials**\. Then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code using the phone keypad\.

## Create the Required IAM Roles \(CLI or API Only\)<a name="dp-iam-roles-new"></a>

AWS Data Pipeline requires IAM roles to determine what actions your pipelines can perform and what resources it can access\. Additionally, when your pipeline creates a resource, such as an EC2 instance or EMR cluster, IAM roles determine what actions your applications can perform and what resources they can access\.

The AWS Data Pipeline console creates the following roles for you:
+ **DataPipelineDefaultRole** grants AWS Data Pipeline access to your AWS resources
+ **DataPipelineDefaultResourceRole** grants the applications on your EC2 instances access to your AWS resources

If you have used AWS Data Pipeline previously and have existing versions of these IAM roles, you might need to update them\. For more information, see [Update Existing IAM Roles for AWS Data Pipeline](dp-iam-roles.md#dp-iam-existing-accounts)\.

If you are using a CLI or an API and you have not used the AWS Data Pipeline console to create a pipeline previously, you must create these roles manually using AWS Identity and Access Management \(IAM\)\.

**To create the required IAM roles manually**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Create the **DataPipelineDefaultRole** role as follows:

   1. In the navigation pane, choose **Roles**, **Create New Role**\.

   1. On the **Set Role Name** page, for **Role name**, enter `DataPipelineDefaultRole`\.

   1. On the **Select Role Type** page, under **AWS Service Roles**, choose **Select** in the row for **AWS Data Pipeline**\.

   1. On the **Attach Policy** page, select the **AWSDataPipelineRole** policy and choose **Next Step**\.

   1. On the **Review** page, choose **Create Role**\.

1. Create the **DataPipelineDefaultResourceRole** role as follows:

   1. In the navigation pane, choose **Roles**, **Create New Role**\.

   1. On the **Set Role Name** page, for **Role name**, enter `DataPipelineDefaultResourceRole`\.

   1. On the **Select Role Type** page, under **AWS Service Roles**, choose **Select** in the row for **Amazon EC2 Role for Data Pipeline**\.

   1. On the **Attach Policy** page, select the **AmazonEC2RoleforDataPipelineRole** policy and choose **Next Step**\.

   1. On the **Review** page, choose **Create Role**\.

Alternatively, you can create and use custom roles\. For more information about how to specify custom roles for an `EmrCluster` object, see [Specify custom IAM roles](emrcluster-example2-disable-encryption.md#example4)\.

## Assign a Managed Policy with PassRole to Predefined IAM Roles<a name="dp-iam-create-user-groups"></a>

All users of AWS Data Pipeline in your account must have `"Action":"iam:PassRole"` permissions to **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** predefined roles, or to any other custom roles that you use to access AWS Data Pipeline\. 

For convenience, you can create a user group for those users and attach a managed policy, **AWSDataPipeline\_FullAccess**, to it\. This managed policy allows users to have `"Action":"iam:PassRole"` permissions to roles used with AWS Data Pipeline\.

In this task, you create a user group, and then attach a **AWSDataPipeline\_FullAccess** managed policy to this group\. 

**To create a user group `DataPipelineDevelopers` and attach the **AWSDataPipeline\_FullAccess** policy**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Groups**, **Create New Group**\.

1. For **Group Name**, type the name of the group, `DataPipelineDevelopers`\. Choose **Next Step**\.

1. In the list of policies, select the **AWSDataPipeline\_FullAccess** managed policy\. Choose **Next Step**\.

1. Choose **Create Group**\.

1. Add users to the group\. Choose **Groups** and then choose the `DataPipelineDevelopers` group\. Choose the **Users** tab and then choose **Add Users to Group**\. Select the users you want to add and then choose **Add Users to Group**\.

## Assign an Inline Policy with PassRole to Custom IAM Roles<a name="dp-iam-inline-policy-passRole"></a>

Instead of using the **AWSDataPipeline\_FullAccess** managed policy with two predefined roles for AWS Data Pipeline, you can create two kinds of custom roles for AWS Data Pipeline, and attach an inline policy that has `"Action":"iam:PassRole"` to each of the roles\. 

Create these two types of custom roles: 
+ A custom role used to launch Amazon EMR clusters through AWS Data Pipeline\. This role may be similar to the predefined role **DataPipelineDefaultRole**, or it may have fewer permissions than the predefined role\. However, this custom role must have the **trust relationship** with the two services it uses, Amazon EMR and AWS Data Pipeline, as follows: 

  ```
  "Effect": "Allow",
        "Principal": {
          "Service": "datapipeline.amazonaws.com"
          "Service": "elasticmapreduce.amazonaws.com"
   
        }
  ```
+ A custom role used to launch Amazon EC2 clusters through AWS Data Pipeline\. This role may be similar to the predefined role **DataPipelineDefaultResourceRole**, or it may have fewer permissions than the predefined role\. However, this custom role must have the **trust relationship** with the Amazon EC2 service, as follows:

  ```
  "Effect": "Allow",
        "Principal": {
          "Service": "ec2.amazonaws.com"
   
        }
  ```

For more information, see [Editing the Trust Relationship for an Existing Role](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/edit_trust.html)\.

For more information about how to specify custom roles for an `EmrCluster` object, see [Specify custom IAM roles](emrcluster-example2-disable-encryption.md#example4)\.

To create your own inline policy for any `CUSTOM_ROLE` role that has access to AWS Data Pipeline, use this example of the inline policy as a guideline\. The `"Action":"iam:PassRole"` is specified for `CUSTOM_ROLE`\.

```
{ 
            "Version": 
            "2012-10-17", 
            "Statement": [ 
             { 
              "Action": [ 
                  "s3:List*", 
                  "dynamodb:DescribeTable",
                    ... 
                    ... 
                    ], 
               "Action": "iam:PassRole", 
               "Effect": "Allow", 
               "Resource": [ 
                   "arn:aws:iam::*:role/CUSTOM_ROLE"
                 ] 
               }}
```