# Copy MySQL Data Using the AWS Data Pipeline Console<a name="dp-copydata-mysql-console"></a>

You can create a pipeline to copy data from a MySQL table to a file in an Amazon S3 bucket\.

**Topics**
+ [Create the Pipeline](#dp-copydata-mysql-define-objects-console)
+ [Save and Validate Your Pipeline](#dp-copydata-mysql-save-pipeline-console)
+ [Verify Your Pipeline Definition](#dp-copydata-mysql-verify-pipeline-console)
+ [Activate Your Pipeline](#dp-copydata-mysql-activate-pipeline-console)
+ [Monitor the Pipeline Runs](#dp-copydata-mysql-execution-pipeline-console)
+ [\(Optional\) Delete Your Pipeline](#dp-copydata-mysql-delete-pipeline-console)

## Create the Pipeline<a name="dp-copydata-mysql-define-objects-console"></a>

First, create the pipeline\. The pipeline must be created in the same region as your target RDS instance\.

**To create your pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. The first screen that you see depends on whether you've created a pipeline in the current region\.

   1. If you haven't created a pipeline in this region, the console displays an introductory screen\. Choose **Get started now**\.

   1. If you've already created a pipeline in this region, the console displays a page that lists your pipelines for the region\. Choose **Create new pipeline**\.

1. In **Name**, enter a name for your pipeline\.

1. \(Optional\) In **Description**, enter a description for your pipeline\.

1. For **Source**, select **Build using a template**, and then select the following template: **Full copy of RDS MySQL table to S3**\.

1. Under the **Parameters** section, which opened when you selected the template, do the following:

   1. For **DBInstance ID**, enter the DB instance name of the Aurora DB instance you want to use to copy data from the Aurora cluster\.

      To locate the endpoint details for your DB instance, see [Connecting to a DB Instance Running the MySQL Database Engine](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ConnectToInstance.html) in the *Amazon RDS User Guide*\. 

   1. For **RDS MySQL username**, enter the user name you used when you created your MySQL database instance\.

   1. In the **RDS MySQL password** field, enter the password you used when you created your DB instance\.

   1. In the **EC2 instance type** field, enter the instance type for your EC2 instance\.

   1. Click the folder icon next to **Output S3 folder**, select one of your buckets or folders, and then click **Select**\.

1. Under **Schedule**, choose **on pipeline activation**\.

1. Under **Pipeline Configuration**, leave logging enabled\. Choose the folder icon under **S3 location for logs**, select one of your buckets or folders, and then choose **Select**\.

   If you prefer, you can disable logging instead\.

1. Under **Security/Access**, leave **IAM roles** set to **Default**\.

1. Click **Edit in Architect**\.

1. In the left pane, separate the icons by dragging them apart\. This is a graphical representation of your pipeline\. The arrows indicate the connections between the objects\.  
![\[Pipeline configuration\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-tutorial-rdstos3.png)

Next, configure the database name setting, which currently is not present on the available template\.

1. In the left pane, click **RDSDatabase**\.

1. In the right pane, under the rds\_mysql section, for **Add an optional field\.\.\.** choose **Database Name**\.

1. Type the **Database Name** of your target database and add optional fields\.

You can configure the Amazon SNS notification action AWS Data Pipeline performs after the copy activity finishes successfully\.

**To configure the Amazon SNS notification action**

1. In the right pane, click **Activities**\.

1. From **Add an optional field**, select **On Success**\.

1. From the newly added **On Success**, select **Create new: Action**\.

1. In the right pane, click **Others**\.

1. Under `DefaultAction1`, do the following:

   1. Enter a name for your notification \(for example, `CopyDataNotice`\)\.

   1. From **Type**, select **SnsAlarm**\.

   1. In the **Message** field, enter the message content\.

   1. In the **Subject** field, enter the subject line for your notification\.

   1. In the **Topic Arn** field, enter the ARN of your topic\.

   1. Leave **Role** field set to the default value\.

## Save and Validate Your Pipeline<a name="dp-copydata-mysql-save-pipeline-console"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.

## Verify Your Pipeline Definition<a name="dp-copydata-mysql-verify-pipeline-console"></a>

It is important that you verify that your pipeline was correctly initialized from your definitions before you activate it\. 

**To verify your pipeline definition**

1. On the **List Pipelines** page, look for your newly\-created pipeline\.

   AWS Data Pipeline has created a unique **Pipeline ID** for your pipeline definition\. 

   The **Schedule State** column in the row listing your pipeline should show `PENDING`\.

1. Choose the triangle icon next to your pipeline\. A pipeline summary pane below shows the details of your pipeline runs\. Because your pipeline is not yet activated, you are not likely to see any execution details\. However, you will see the configuration of the pipeline definition\.

## Activate Your Pipeline<a name="dp-copydata-mysql-activate-pipeline-console"></a>

Activate your pipeline to start creating and processing runs\. The pipeline starts based on the schedule and period in your pipeline definition\.

**Important**  
If activation succeeds, your pipeline is running and might incur usage charges\. For more information, see [AWS Data Pipeline pricing](http://aws.amazon.com/datapipeline/pricing)\. To stop incurring usage charges for AWS Data Pipeline, delete your pipeline\.

**To activate your pipeline**

1. Choose **Activate**\.

1. In the confirmation dialog box, choose **Close**\.

## Monitor the Pipeline Runs<a name="dp-copydata-mysql-execution-pipeline-console"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline runs**

1. Choose **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then choose **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\. If you created an SNS notification, you should receive email about the successful completion of this task\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

## \(Optional\) Delete Your Pipeline<a name="dp-copydata-mysql-delete-pipeline-console"></a>

To stop incurring charges, delete your pipeline\. Deleting your pipeline deletes the pipeline definition and all associated objects\.

**To delete your pipeline**

1. On the **List Pipelines** page, select your pipeline\.

1. Click **Actions**, and then choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.