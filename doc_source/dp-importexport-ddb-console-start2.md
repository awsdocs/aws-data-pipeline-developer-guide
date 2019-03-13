# Step 1: Create the Pipeline<a name="dp-importexport-ddb-console-start2"></a>

First, create the pipeline\.

**To create the pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. The first screen that you see depends on whether you've created a pipeline in the current region\.

   1. If you haven't created a pipeline in this region, the console displays an introductory screen\. Choose **Get started now**\.

   1. If you've already created a pipeline in this region, the console displays a page that lists your pipelines for the region\. Choose **Create new pipeline**\.

1. In **Name**, enter a name for your pipeline\.

1. \(Optional\) In **Description**, enter a description for your pipeline\.

1. For **Source**, select **Build using a template**, and then select the following template: **Export DynamoDB table to S3**\.

1. Under **Parameters**, set **DynamoDB table name** to the name of your table\. Click the folder icon next to **Output S3 folder**, select one of your Amazon S3 buckets, and then click **Select**\.

1. Under **Schedule**, choose **on pipeline activation**\.

1. Under **Pipeline Configuration**, leave logging enabled\. Choose the folder icon under **S3 location for logs**, select one of your buckets or folders, and then choose **Select**\.

   If you prefer, you can disable logging instead\.

1. Under **Security/Access**, leave **IAM roles** set to **Default**\.

1. Click **Edit in Architect**\.  
![\[Import/export table screen\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-ddb-export-template.png)

Next, configure the Amazon SNS notification actions that AWS Data Pipeline performs depending on the outcome of the activity\.

**To configure the success, failure, and late notification actions**

1. In the right pane, click **Activities**\.

1. From **Add an optional field**, select **On Success**\.

1. From the newly added **On Success**, select **Create new: Action**\.

1. From **Add an optional field**, select **On Fail**\.

1. From the newly added **On Fail**, select **Create new: Action**\.

1. From **Add an optional field**, select **On Late Action**\.

1. From the newly added **On Late Action**, select **Create new: Action**\.

1. In the right pane, click **Others**\.

1. For `DefaultAction1`, do the following:

   1. Change the name to `SuccessSnsAlarm`\.

   1. From **Type**, select `SnsAlarm`\.

   1. In **Topic Arn**, enter the ARN of the topic that you created\. See [ARN resource names for Amazon SNS](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-sns)\.

   1. Enter a subject and a message\.

1. For `DefaultAction2`, do the following:

   1. Change the name to `FailureSnsAlarm`\.

   1. From **Type**, select `SnsAlarm`\.

   1. In **Topic Arn**, enter the ARN of the topic that you created \(see [ARN resource names for Amazon SNS](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-sns)\.

   1. Enter a subject and a message\.

1. For `DefaultAction3`, do the following: 

   1. Change the name to `LateSnsAlarm`\.

   1. From **Type**, select `SnsAlarm`\.

   1. In **Topic Arn**, enter the ARN of the topic that you created \(see [ARN resource names for Amazon SNS](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-sns)\.

   1. Enter a subject and a message\.