# Copy CSV Data Using the AWS Data Pipeline Console<a name="dp-copydata-s3-console"></a>

You can create and use pipelines to copy data from one Amazon S3 bucket to another\.

**Topics**
+ [Create the Pipeline](#dp-copydata-s3-define-objects-console)
+ [Save and Validate Your Pipeline](#dp-copydata-s3-save-pipeline-console)
+ [Activate Your Pipeline](#dp-copydata-s3-activate-pipeline-console)
+ [Monitor the Pipeline Runs](#dp-copydata-s3-execution-pipeline-console)
+ [\(Optional\) Delete Your Pipeline](#dp-copydata-s3-delete-pipeline-console)

## Create the Pipeline<a name="dp-copydata-s3-define-objects-console"></a>

First, create the pipeline\.

**To create the pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. The first screen that you see depends on whether you've created a pipeline in the current region\.

   1. If you haven't created a pipeline in this region, the console displays an introductory screen\. Choose **Get started now**\.

   1. If you've already created a pipeline in this region, the console displays a page that lists your pipelines for the region\. Choose **Create new pipeline**\.

1. In **Name**, enter a name for your pipeline\.

1. \(Optional\) In **Description**, enter a description for your pipeline\.

1. For **Source**, select **Build using Architect**\.

1. Under **Schedule**, choose **on pipeline activation**\.

1. Under **Pipeline Configuration**, leave logging enabled\. Choose the folder icon under **S3 location for logs**, select one of your buckets or folders, and then choose **Select**\.

   If you prefer, you can disable logging instead\.

1. Under **Security/Access**, leave **IAM roles** set to **Default**\.

1. Click **Edit in Architect**\.

Next, define the `Activity` object in your pipeline definition\. When you define the `Activity` object, you also define the objects that AWS Data Pipeline must use to perform this activity\.

**To configure the activity for your pipeline**

1. Click **Add activity**\.

1. In the **Activities** pane:

   1. In the **Name** field, enter a name for the activity, for example, `copy-myS3-data`\.

   1. From **Type**, select **CopyActivity**\.

   1. From **Output**, select **Create new: DataNode**\.

   1. From **Schedule**, select **Create new: Schedule**\.

   1. From **Input**, select **Create new: DataNode**\.

   1. From **Add an optional field**, select **Runs On**\.

   1. From the newly added **Runs On**, select **Create new: Resource**\.

   1. From **Add an optional field**, select **On Success**\.

   1. From the newly added **On Success**, select **Create new: Action**\.  
![\[Activities pane\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-canvas-copyS3.png)

1. In the left pane, separate the icons by dragging them apart\. This is a graphical representation of your pipeline\. The arrows indicate the connections between the objects\.  
![\[Graphical representation of the pipeline\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-copy-s3-data.png)

Next, configure the input and the output data nodes for your pipeline\.

**To configure the input and output data nodes for your pipeline**

1. In the right pane, click **DataNodes**\.

1. For `DefaultDataNode1`, which represents your data source, do the following:

   1. Enter a name for your input node \(for example, `MyS3Input`\)\.

   1. From **Type**, select **S3DataNode**\.

   1. From **Schedule**, select your schedule \(for example, `copy-S3data-schedule`\)\.

   1. From **Add an optional field**, select **File Path**\.

   1. In the **File Path** field, enter the path in Amazon S3 for your data source\.

1. For `DefaultDataNode2`, which represents your data target, do the following:

   1. Enter a name for your output node \(for example, `MyS3Output`\)\.

   1. From **Type**, select **S3DataNode**\. 

   1. From **Schedule**, select your schedule \(for example, `copy-S3data-schedule`\)\.

   1. From **Add an optional field**, select **File Path**\.

   1. In the **File Path** field, enter the path in Amazon S3 for your data target\.

Next, configure the resource AWS Data Pipeline must use to perform the copy activity\.

**To configure the resource**

1. In the right pane, click **Resources**\.

1. Enter a name for your resource \(for example, `CopyDataInstance`\)\.

1. From **Type**, select **Ec2Resource**\.

1. From **Schedule**, select your schedule \(for example, `copy-S3data-schedule`\)\.

1. Leave **Resource Role** and **Role** set to their default values\.

   If you have created your own IAM roles, you can select them if you prefer\.

Next, configure the Amazon SNS notification action that AWS Data Pipeline performs after the copy activity finishes successfully\.

**To configure the notification action**

1. In the right pane, click **Others**\.

1. Under `DefaultAction1`, do the following:

   1. Enter a name for your notification \(for example, `CopyDataNotice`\)\.

   1. From **Type**, select **SnsAlarm**\.

   1. In the **Subject** field, enter the subject line for your notification\.

   1. In the **Topic Arn** field, enter the ARN of your topic\.

   1. In the **Message** field, enter the message content\.

   1. Leave **Role** field set to the default value\.

## Save and Validate Your Pipeline<a name="dp-copydata-s3-save-pipeline-console"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.

## Activate Your Pipeline<a name="dp-copydata-s3-activate-pipeline-console"></a>

Activate your pipeline to start creating and processing runs\. The pipeline starts based on the schedule and period in your pipeline definition\.

**Important**  
If activation succeeds, your pipeline is running and might incur usage charges\. For more information, see [AWS Data Pipeline pricing](http://aws.amazon.com/datapipeline/pricing)\. To stop incurring usage charges for AWS Data Pipeline, delete your pipeline\.

**To activate your pipeline**

1. Choose **Activate**\.

1. In the confirmation dialog box, choose **Close**\.

## Monitor the Pipeline Runs<a name="dp-copydata-s3-execution-pipeline-console"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline runs**

1. Choose **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then choose **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\. If you created an SNS notification, you should receive email about the successful completion of this task\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

## \(Optional\) Delete Your Pipeline<a name="dp-copydata-s3-delete-pipeline-console"></a>

To stop incurring charges, delete your pipeline\. Deleting your pipeline deletes the pipeline definition and all associated objects\.

**To delete your pipeline**

1. On the **List Pipelines** page, select your pipeline\.

1. Click **Actions**, and then choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.