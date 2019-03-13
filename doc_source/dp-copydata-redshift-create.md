# Copy Data to Amazon Redshift Using the AWS Data Pipeline Console<a name="dp-copydata-redshift-create"></a>

You can create a pipeline to copy data from Amazon S3 to Amazon Redshift\. You'll create a new table in Amazon Redshift, and then use AWS Data Pipeline to transfer data to this table from a public Amazon S3 bucket, which contains sample input data in CSV format\. The logs are saved to an Amazon S3 bucket that you own\.

Amazon S3 is a web service that enables you to store data in the cloud\. For more information, see the [Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)\. 

Amazon Redshift is a data warehouse service in the cloud\. For more information, see the [Amazon Redshift Cluster Management Guide](https://docs.aws.amazon.com/redshift/latest/mgmt/)\.

**Prerequisites**  
Before you start this tutorial, complete the prerequisites described in [Before You Begin: Configure COPY Options and Load Data](dp-learn-copy-redshift.md) and [Set up Pipeline, Create a Security Group, and Create an Amazon Redshift Cluster](dp-copydata-redshift-prereq.md)\.

**Topics**
+ [Create the Pipeline](#dp-copydata-redshift-define-objects-console)
+ [Save and Validate Your Pipeline](#dp-copydata-redshift-save-pipeline-console)
+ [Activate Your Pipeline](#dp-copydata-redshift-activate-pipeline-console)
+ [Monitor the Pipeline Runs](#dp-copydata-redshift-execution-pipeline-console)
+ [\(Optional\) Delete Your Pipeline](#dp-copydata-redshift-delete-pipeline-console)

## Create the Pipeline<a name="dp-copydata-redshift-define-objects-console"></a>

First, create the pipeline\.

This pipeline relies on the options of the [Syntax](dp-object-redshiftcopyactivity.md#redshiftcopyactivity-syntax)\.

It uses the **Copy to Redshift** template in the AWS Data Pipeline console\. For information about this template, see [Load Data from Amazon S3 into Amazon Redshift](dp-template-s3redshift.md)\.

**To create the pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. The first screen that you see depends on whether you've created a pipeline in the current region\.

   1. If you haven't created a pipeline in this region, the console displays an introductory screen\. Choose **Get started now**\.

   1. If you've already created a pipeline in this region, the console displays a page that lists your pipelines for the region\. Choose **Create new pipeline**\.

1. In **Name**, enter a name for your pipeline\.

1. \(Optional\) In **Description**, enter a description for your pipeline\.

1. For **Source**, select **Build using a template**, and then select the following template: **Load data from S3 into Redshift**\.

1. Under **Parameters**, provide information about your input folder in Amazon S3 and the Amazon Redshift database that you created\.

1. Under **Schedule**, choose **on pipeline activation**\.

1. Under **Pipeline Configuration**, leave logging enabled\. Choose the folder icon under **S3 location for logs**, select one of your buckets or folders, and then choose **Select**\.

   If you prefer, you can disable logging instead\.

1. Under **Security/Access**, leave **IAM roles** set to **Default**\.

1. Click **Activate**\.

   If you prefer, you can choose **Edit in Architect** to modify this pipeline\. For example, you can add preconditions\.

## Save and Validate Your Pipeline<a name="dp-copydata-redshift-save-pipeline-console"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.

## Activate Your Pipeline<a name="dp-copydata-redshift-activate-pipeline-console"></a>

Activate your pipeline to start creating and processing runs\. The pipeline starts based on the schedule and period in your pipeline definition\.

**Important**  
If activation succeeds, your pipeline is running and might incur usage charges\. For more information, see [AWS Data Pipeline pricing](http://aws.amazon.com/datapipeline/pricing)\. To stop incurring usage charges for AWS Data Pipeline, delete your pipeline\.

**To activate your pipeline**

1. Choose **Activate**\.

1. In the confirmation dialog box, choose **Close**\.

## Monitor the Pipeline Runs<a name="dp-copydata-redshift-execution-pipeline-console"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline runs**

1. Choose **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then choose **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\. If you created an SNS notification, you should receive email about the successful completion of this task\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

## \(Optional\) Delete Your Pipeline<a name="dp-copydata-redshift-delete-pipeline-console"></a>

To stop incurring charges, delete your pipeline\. Deleting your pipeline deletes the pipeline definition and all associated objects\.

**To delete your pipeline**

1. On the **List Pipelines** page, select your pipeline\.

1. Click **Actions**, and then choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.