# Getting Started with AWS Data Pipeline<a name="dp-getting-started"></a>

AWS Data Pipeline helps you sequence, schedule, run, and manage recurring data processing workloads reliably and cost\-effectively\. This service makes it easy for you to design extract\-transform\-load \(ETL\) activities using structured and unstructured data, both on\-premises and in the cloud, based on your business logic\.

To use AWS Data Pipeline, you create a *pipeline definition* that specifies the business logic for your data processing\. A typical pipeline definition consists of [activities](dp-concepts-activities.md) that define the work to perform, [data nodes](dp-concepts-datanodes.md) that define the location and type of input and output data, and a [schedule](dp-concepts-schedules.md) that determines when the activities are performed\.

In this tutorial, you run a shell command script that counts the number of GET requests in Apache web server logs\. This pipeline runs every 15 minutes for an hour, and writes output to Amazon S3 on each iteration\.

**Prerequisites**  
Before you begin, complete the tasks in [Setting up for AWS Data Pipeline](dp-get-setup.md)\.

**Pipeline Objects**  
The pipeline uses the following objects:

[ShellCommandActivity](dp-object-shellcommandactivity.md)  
Reads the input log file and counts the number of errors\.

[S3DataNode](dp-object-s3datanode.md) \(input\)  
The S3 bucket that contains the input log file\.

[S3DataNode](dp-object-s3datanode.md) \(output\)  
The S3 bucket for the output\.

[Ec2Resource](dp-object-ec2resource.md)  
The compute resource that AWS Data Pipeline uses to perform the activity\.  
Note that if you have a large amount of log file data, you can configure your pipeline to use an EMR cluster to process the files instead of an EC2 instance\.

[Schedule](dp-object-schedule.md)  
Defines that the activity is performed every 15 minutes for an hour\.

**Topics**
+ [Create the Pipeline](#dp-getting-started-create)
+ [Monitor the Running Pipeline](#dp-getting-started-monitor)
+ [View the Output](#dp-getting-started-output)
+ [Delete the Pipeline](#dp-getting-started-delete)

## Create the Pipeline<a name="dp-getting-started-create"></a>

The quickest way to get started with AWS Data Pipeline is to use a pipeline definition called a *template*\.

**To create the pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. From the navigation bar, select a region\. You can select any region that's available to you, regardless of your location\. Many AWS resources are specific to a region, but AWS Data Pipeline enables you to use resources that are in a different region than the pipeline\.

1. The first screen that you see depends on whether you've created a pipeline in the current region\.

   1. If you haven't created a pipeline in this region, the console displays an introductory screen\. Choose **Get started now**\.

   1. If you've already created a pipeline in this region, the console displays a page that lists your pipelines for the region\. Choose **Create new pipeline**\.

1. In **Name**, enter a name for your pipeline\.

1. \(Optional\) In **Description**, enter a description for your pipeline\.

1. For **Source**, select **Build using a template**, and then select the following template: **Getting Started using ShellCommandActivity**\.

1. Under the **Parameters** section, which opened when you selected the template, leave **S3 input folder** and **Shell command to run** with their default values\. Click the folder icon next to **S3 output folder**, select one of your buckets or folders, and then click **Select**\.

1. Under **Schedule**, leave the default values\. When you activate the pipeline the pipeline runs start, and then continue every 15 minutes for an hour\.

   If you prefer, you can select **Run once on pipeline activation** instead\.

1. Under **Pipeline Configuration**, leave logging enabled\. Choose the folder icon under **S3 location for logs**, select one of your buckets or folders, and then choose **Select**\.

   If you prefer, you can disable logging instead\.

1. Under **Security/Access**, leave **IAM roles** set to **Default**\.

1. Click **Activate**\.

   If you prefer, you can choose **Edit in Architect** to modify this pipeline\. For example, you can add preconditions\.

## Monitor the Running Pipeline<a name="dp-getting-started-monitor"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline**

1. Click **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then click **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

## View the Output<a name="dp-getting-started-output"></a>

Open the Amazon S3 console and navigate to your bucket\. If you ran your pipeline every 15 minutes for an hour, you'll see four time\-stamped subfolders\. Each subfolder contains output in a file named `output.txt`\. Because we ran the script on the same input file each time, the output files are identical\.

## Delete the Pipeline<a name="dp-getting-started-delete"></a>

To stop incurring charges, delete your pipeline\. Deleting your pipeline deletes the pipeline definition and all associated objects\.

**To delete your pipeline**

1. On the **List Pipelines** page, select your pipeline\.

1. Click **Actions**, and then choose **Delete**\.

1. When prompted for confirmation, choose **Delete**\.

If you are finished with the output from this tutorial, delete the output folders from your Amazon S3 bucket\.