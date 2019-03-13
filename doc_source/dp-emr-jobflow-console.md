# Launch a Cluster Using the AWS Data Pipeline Console<a name="dp-emr-jobflow-console"></a>

You can create a pipeline to launch a cluster to analyze web logs or perform analysis of scientific data\.

**Topics**
+ [Create the Pipeline](#dp-emr-jobflow-define-objects-console)
+ [Save and Validate Your Pipeline](#dp-emr-jobflow-save-pipeline-console)
+ [Activate Your Pipeline](#dp-emr-jobflow-activate-pipeline-console)
+ [Monitor the Pipeline Runs](#dp-emr-jobflow-execution-pipeline-console)
+ [\(Optional\) Delete Your Pipeline](#dp-emr-jobflow-delete-pipeline-console)

## Create the Pipeline<a name="dp-emr-jobflow-define-objects-console"></a>

First, create the pipeline\.

**To create your pipeline**

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

Next, add an activity to your pipeline definition\. This also defines the other objects that AWS Data Pipeline must use to perform this activity\.

**To configure the activity**

1. Click **Add activity**\.

1. In the **Activities** pane:

   1. In the **Name** field, enter the name for the activity \(for example, `MyEMRActivity`\)\.

   1. From **Type**, select **EmrActivity**\.

   1. In **Step**, enter:

      ```
      /home/hadoop/contrib/streaming/hadoop-streaming.jar,-input,s3n://elasticmapreduce/samples/wordcount/input,-output,s3://example-bucket/wordcount/output/#{@scheduledStartTime},-mapper,s3n://elasticmapreduce/samples/wordcount/wordSplitter.py,-reducer,aggregate
      ```

   1. In **Add an optional field**, select **Runs On**\. Set the value to **Create new: EmrCluster**\.

   1. In **Add an optional field**, select **On Success**\. Set the value to **Create new: Action**\.  
![\[Graphical representation of the pipeline\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-emr-jobflow-template.png)

Next, configure the resource that AWS Data Pipeline uses to perform the Amazon EMR job\. 

**To configure the resource**

1. In the right pane, choose **Resources**\.

1. In the **Name** field, enter the name for your Amazon EMR cluster \(for example, `MyEMRCluster`\)\.

1. Leave **Type** set to **EmrCluster**\.

1. \[EC2\-VPC\] \(Optional\) From **Add an optional field**, select **Subnet Id**\. Set the value to the ID of the subnet\.

1. \(Optional\) From **Add an optional field**, select **Enable Debugging**\. Set the value to **true**\.
**Note**  
This option can incur extra costs because of log data storage\. Use this option selectively, such as for prototyping and troubleshooting\.

1. \(Optional\) In the right pane, choose **Others**\. Under `Default`, from **Add an optional field**, select **Pipeline Log Uri**\. Set the value to an Amazon S3 bucket for the Amazon EMR logs\. For example, **s3://examples\-bucket/emrlogs**\.
**Note**  
This option can incur extra costs because of log file storage\. Use this option selectively, such as for prototyping and troubleshooting\.

Next, configure the Amazon SNS notification action that AWS Data Pipeline performs after the Amazon EMR job finishes successfully\.

**To configure the notification action**

1. In the right pane, click **Others**\.

1. Under `DefaultAction1`, do the following:

   1. Enter the name for your notification \(for example, `MyEMRJobNotice`\)\.

   1. From **Type**, select **SnsAlarm**\.

   1. In the **Subject** field, enter the subject line for your notification\.

   1. In the **Topic Arn** field, enter the ARN of your topic \(see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\)\.

   1. In **Message**, enter the message content\.

   1. Leave **Role** set to the default value\.

## Save and Validate Your Pipeline<a name="dp-emr-jobflow-save-pipeline-console"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.

## Activate Your Pipeline<a name="dp-emr-jobflow-activate-pipeline-console"></a>

Activate your pipeline to start creating and processing runs\. The pipeline starts based on the schedule and period in your pipeline definition\.

**Important**  
If activation succeeds, your pipeline is running and might incur usage charges\. For more information, see [AWS Data Pipeline pricing](http://aws.amazon.com/datapipeline/pricing)\. To stop incurring usage charges for AWS Data Pipeline, delete your pipeline\.

**To activate your pipeline**

1. Choose **Activate**\.

1. In the confirmation dialog box, choose **Close**\.

## Monitor the Pipeline Runs<a name="dp-emr-jobflow-execution-pipeline-console"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline runs**

1. Choose **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then choose **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\. If you created an SNS notification, you should receive email about the successful completion of this task\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

## \(Optional\) Delete Your Pipeline<a name="dp-emr-jobflow-delete-pipeline-console"></a>

To stop incurring charges, delete your pipeline\. Deleting your pipeline deletes the pipeline definition and all associated objects\.

**To delete your pipeline**

1. On the **List Pipelines** page, select your pipeline\.

1. Click **Actions**, and then choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.