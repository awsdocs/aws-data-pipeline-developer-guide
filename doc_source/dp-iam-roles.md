# IAM Roles for AWS Data Pipeline<a name="dp-iam-roles"></a>

AWS Data Pipeline requires IAM roles to determine what actions your pipelines can perform and what resources it can access\. Additionally, when a pipeline creates a resource, such as an EC2 instance or EMR cluster, IAM roles determine what actions your applications can perform and what resources they can access\.

The AWS Data Pipeline console creates the following roles for you:
+ **DataPipelineDefaultRole** \- Grants AWS Data Pipeline access to your AWS resources
+ **DataPipelineDefaultResourceRole** \- Grants your applications access to your AWS resources

If you are using a CLI or an API and you have not used the AWS Data Pipeline console to create a pipeline previously, you must create these roles manually using AWS Identity and Access Management \(IAM\)\. For more information, see [Create the Required IAM Roles \(CLI or API Only\)](dp-get-setup.md#dp-iam-roles-new)\.

Alternatively, you can create custom roles\. For an example of how to specify these roles for an `EmrCluster` object, see [Specify custom IAM roles](emrcluster-example2-disable-encryption.md#example4)\.

## Update Existing IAM Roles for AWS Data Pipeline<a name="dp-iam-existing-accounts"></a>

If the **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** roles were created using inline policies instead of managed policies, the AWS account owner can update them to use managed policies\. After you update your roles to use an AWS managed policy, they will receive future updates automatically\.

Use the following procedure to update the **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** roles\.

**To update your existing IAM roles using managed policies**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Update the **DataPipelineDefaultRole** role as follows:

   1. In the navigation pane, click **Roles**, and then click the row for the **DataPipelineDefaultRole** role\.

   1. Under **Permissions**, click **Remove Policy** for the inline policy\. When prompted for confirmation, click **Remove**\.

   1. Under **Permissions**, click **Attach Policy**\.

   1. On the **Attach Policy** page, click the box next to the **AWSDataPipelineRole** policy, and then click **Attach Policy**\.

1. Update the **DataPipelineDefaultResourceRole** role as follows:

   1. In the navigation pane, click **Roles**, and then click the row for the **DataPipelineDefaultResourceRole** role

   1. Under **Permissions**, click **Remove Policy** for the inline policy\. When prompted for confirmation, click **Remove**\.

   1. Under **Permissions**, click **Attach Policy**\.

   1. On the **Attach Policy** page, click the box next to the **AmazonEC2RoleforDataPipelineRole** policy, and then click **Attach Policy**\.

If you prefer to maintain the inline policy yourself, you can do so as follows\.

**To update your existing IAM roles using inline policies**

1. Update `DataPipelineDefaultRole` to use the following policy:

   ```
   {
      "Version": "2012-10-17",
      "Statement": [{
          "Effect": "Allow",
          "Action": [
            "cloudwatch:*",
            "datapipeline:DescribeObjects",
            "datapipeline:EvaluateExpression",
            "dynamodb:BatchGetItem",
            "dynamodb:DescribeTable",
            "dynamodb:GetItem",
            "dynamodb:Query",
            "dynamodb:Scan",
            "dynamodb:UpdateTable",
            "ec2:AuthorizeSecurityGroupIngress",
            "ec2:CancelSpotInstanceRequests",
            "ec2:CreateSecurityGroup",
            "ec2:CreateTags",
            "ec2:DeleteTags",
            "ec2:Describe*",
            "ec2:ModifyImageAttribute",
            "ec2:ModifyInstanceAttribute",
            "ec2:RequestSpotInstances",
            "ec2:RunInstances",
            "ec2:StartInstances",
            "ec2:StopInstances",
            "ec2:TerminateInstances",
            "ec2:AuthorizeSecurityGroupEgress", 
            "ec2:DeleteSecurityGroup", 
            "ec2:RevokeSecurityGroupEgress", 
            "ec2:DescribeNetworkInterfaces", 
            "ec2:CreateNetworkInterface", 
            "ec2:DeleteNetworkInterface", 
            "ec2:DetachNetworkInterface",
            "elasticmapreduce:*",
            "iam:GetInstanceProfile",
            "iam:GetRole",
            "iam:GetRolePolicy",
            "iam:ListAttachedRolePolicies",
            "iam:ListRolePolicies",
            "iam:ListInstanceProfiles",
            "iam:PassRole",
            "rds:DescribeDBInstances",
            "rds:DescribeDBSecurityGroups",
            "redshift:DescribeClusters",
            "redshift:DescribeClusterSecurityGroups",
            "s3:CreateBucket",
            "s3:DeleteObject",
            "s3:Get*",
            "s3:List*",
            "s3:Put*",
            "sdb:BatchPutAttributes",
            "sdb:Select*",
            "sns:GetTopicAttributes",
            "sns:ListTopics",
            "sns:Publish",
            "sns:Subscribe",
            "sns:Unsubscribe",
            "sqs:CreateQueue", 
            "sqs:Delete*", 
            "sqs:GetQueue*", 
            "sqs:PurgeQueue", 
            "sqs:ReceiveMessage" 
          ],
          "Resource": ["*"]
        },
        {
          "Effect": "Allow",
          "Action": "iam:CreateServiceLinkedRole",
          "Resource": "*",
          "Condition": {
            "StringLike": {
                "iam:AWSServiceName": ["elasticmapreduce.amazonaws.com","spot.amazonaws.com"]
            }
          }
        }]
    }
   ```

1. Update `DataPipelineDefaultRole` to use the following trusted entities list:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "elasticmapreduce.amazonaws.com",
             "datapipeline.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Update `DataPipelineDefaultResourceRole` to use the following policy:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [{
         "Effect": "Allow",
         "Action": [
           "cloudwatch:*",
           "datapipeline:*",
           "dynamodb:*",
           "ec2:Describe*",
           "elasticmapreduce:AddJobFlowSteps",
           "elasticmapreduce:Describe*",
           "elasticmapreduce:ListInstance*",
           "rds:Describe*",
           "redshift:DescribeClusters",
           "redshift:DescribeClusterSecurityGroups",
           "s3:*",
           "sdb:*",
           "sns:*",
           "sqs:*"
         ],
         "Resource": ["*"]
       }]
   }
   ```

1. Update `DataPipelineDefaultResourceRole` to use the following trusted entities list:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "ec2.amazonaws.com"         
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

## Change Roles on Existing Pipelines<a name="dp-iam-change-console"></a>

If you have a custom role and you would like to change roles for existing pipelines by editing the pipeline in the AWS Data Pipeline console:

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. Select the pipeline you wish to edit by clicking the pipeline ID\.

1. Choose **Edit Pipeline**\.

1. Choose the **Others** dropdown and enter the appropriate **Role** and **Resource Role**\.