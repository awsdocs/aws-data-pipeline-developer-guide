# Creating Pipelines Using the Console Manually<a name="dp-console-manual"></a>

You can create a pipeline using the AWS Data Pipeline console without the assistance of templates\. The example pipeline uses AWS Data Pipeline to copy a CSV from one Amazon S3 bucket to another on a schedule\.

**Prerequisites**  
An Amazon S3 bucket for the file copy source and destination used in the procedure\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.

**Topics**
+ [Create the Pipeline Definition](#dp-console-manual-create)
+ [Define Activities](#dp-console-manual-activity)
+ [Configure the Schedule](#dp-console-manual-schedule)
+ [Configure Data Nodes](#dp-console-manual-data)
+ [Configure Resources](#dp-console-manual-resources)
+ [Validate and Save the Pipeline](#dp-console-manual-validate)
+ [Activate the Pipeline](#dp-console-manual-activate)

## Create the Pipeline Definition<a name="dp-console-manual-create"></a>

Complete the initial pipeline creation screen to create the pipeline definition\.

**To create your pipeline definition**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. Click **Get started now** \(if this is your first pipeline\) or **Create new pipeline**\.

1. In **Name**, enter a name for the pipeline \(for example, `CopyMyS3Data`\)\.

1. In **Description**, enter a description\.

1. <a name="step_cjr_wb4_4q"></a>Choose a pipeline definition **Source**\. You can use a template, import an existing JSON\-based pipeline definition from your local file system or an Amazon S3 bucket, or create a pipeline interactively on the Architect page\.

   Templates provide common scenarios encountered in AWS Data Pipeline\. You can customize a template to fit your needs by filling out the associated parameter values\. 
**Note**  
If your existing pipeline definition contains more than one schedule, the schedule is not visible in the **Create Pipeline** page, but you can continue to the **Architect** page to view your schedules\.

1. In **Pipeline Configuration**, if you choose to enable logging, select a bucket in Amazon S3 to store logs for this pipeline\.

1. Leave the **Schedule** fields set to their default values\.

1. Leave **IAM roles** set to **Default**\.

   Alternatively, if you created your own IAM roles and would like to use them, click **Custom** and select them from the **Pipeline role** and **EC2 instance role** lists\.

1. Click **Create**\.

## Define Activities<a name="dp-console-manual-activity"></a>

Add `Activity` objects to your pipeline definition\. When you define an `Activity` object, you must also define the objects that AWS Data Pipeline needs to perform this activity\. 

**To define activities for your pipeline**

1. On the pipeline page, click **Add activity**\.

1. From the **Activities** pane, in **Name**, enter a name for the activity \(for example, `copy-myS3-data`\)\.

1. In **Type**, select **CopyActivity**\.

1. In **Schedule**, select **Create new: Schedule**\.

1. In **Input**, select **Create new: DataNode**\.

1. In **Output**, select **Create new: DataNode**\.

1. In **Add an optional field**, select **RunsOn**\.

1. In **Runs On**, select **Create new: Resource**\.

1. In the left pane, separate the icons by dragging them apart\.

   This is a graphical representation of the pipeline\. The arrows indicate the connection between the various objects\. Your pipeline should look similar to the following image\.  
![\[New pipeline with an activity and two data nodes\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-create-pipeline-console.png)

## Configure the Schedule<a name="dp-console-manual-schedule"></a>

Configure the run date and time for your pipeline\. AWS Data Pipeline supports the date and time expressed in "YYYY\-MM\-DDTHH:MM:SS" format in UTC/GMT only\.

**To configure the run date and time for your pipeline**

1. On the pipeline page, in the right pane, expand the **Schedules** pane\.

1. Enter a schedule name for this activity \(for example, `copy-myS3-data-schedule`\)\.

1. In **Start Date Time**, select the date from the calendar, and then enter the time to start the activity\.

1. In **Period**, enter the duration for the activity \(for example, `1`\), and then select the period category \(for example, `Days`\)\.

1. \(Optional\) To specify the date and time to end the activity, in **Add an optional field**, select **End Date Time**, and enter the date and time\.

   To get your pipeline to launch immediately, set **Start Date Time** to a date one day in the past\. AWS Data Pipeline then starts launching the "past due" runs immediately in an attempt to address what it perceives as a backlog of work\. This backfilling means that you don't have to wait an hour to see AWS Data Pipeline launch its first cluster\. 

## Configure Data Nodes<a name="dp-console-manual-data"></a>

Configure the input and the output data nodes for your pipeline\.

**To configure the input and output data nodes of your pipeline**

1. On the pipeline page, in the right pane, click **DataNodes**\.

1. Under `DefaultDataNode1`, in **Name**, enter a name for the Amazon S3 bucket to use as your input node \(for example, `MyS3Input`\)\.

1. In **Type**, select **S3DataNode**\. 

1. In **Schedule**, select **copy\-myS3\-data\-schedule**\.

1. In **Add an optional field**, select **File Path**\.

1. In **File Path**, enter the path to your Amazon S3 bucket \(for example, `s3://my-data-pipeline-input/data`\)\.

1. Under `DefaultDataNode2`, in **Name**, enter a name for the Amazon S3 bucket to use as your output node \(for example, `MyS3Output`\)\.

1. In **Type**, select **S3DataNode**\. 

1. In **Schedule**, select **copy\-myS3\-data\-schedule**\.

1. In **Add an optional field**, select **File Path**\.

1. In **File Path**, enter the path to your Amazon S3 bucket \(for example, `s3://my-data-pipeline-output/data`\)\.

## Configure Resources<a name="dp-console-manual-resources"></a>

Configure the resource that AWS Data Pipeline must use to perform the copy activity, an EC2 instance\.

**To configure an EC2 instance for your pipeline**

1. On the pipeline page, in the right pane, click **Resources**\.

1. In **Name**, enter a name for your resource \(for example, `CopyDataInstance`\)\.

1. In **Type**, select **Ec2Resource**\.

1. \[EC2\-VPC\] In **Add an optional field**, select **Subnet Id**\.

1. \[EC2\-VPC\] In **Subnet Id**, enter the ID of the subnet\.

1. In **Schedule**, select **copy\-myS3\-data\-schedule**\.

1. Leave **Role** and **Resource Role** set to their default values\. 

   Alternatively, if you created your own IAM roles and would like to use them, click **Custom** and select them from the **Pipeline role** and **EC2 instance role** lists\.

## Validate and Save the Pipeline<a name="dp-console-manual-validate"></a>

You can save your pipeline definition at any point during the creation process\. As soon as you save your pipeline definition, AWS Data Pipeline looks for syntax errors and missing values in your pipeline definition\. If your pipeline is incomplete or incorrect, AWS Data Pipeline generates validation errors and warnings\. Warning messages are informational only, but you must fix any error messages before you can activate your pipeline\.

**To save and validate your pipeline**

1. Choose **Save pipeline**\.

1. AWS Data Pipeline validates your pipeline definition and returns either success or error or warning messages\. If you get an error message, choose **Close** and then, in the right pane, choose **Errors/Warnings**\.

1. The **Errors/Warnings** pane lists the objects that failed validation\. Choose the plus \(**\+**\) sign next to the object names and look for an error message in red\.

1. When you see an error message, go to the specific object pane where you see the error and fix it\. For example, if you see an error message in the **DataNodes** object, go to the **DataNodes** pane to fix the error\.

1. After you fix the errors listed in the **Errors/Warnings** pane, choose **Save Pipeline**\.

1. Repeat the process until your pipeline validates successfully\.

## Activate the Pipeline<a name="dp-console-manual-activate"></a>

Activate your pipeline to start creating and processing runs\. The pipeline starts based on the schedule and period in your pipeline definition\.

**Important**  
If activation succeeds, your pipeline is running and might incur usage charges\. For more information, see [AWS Data Pipeline pricing](http://aws.amazon.com/datapipeline/pricing)\. To stop incurring usage charges for AWS Data Pipeline, delete your pipeline\.

**To activate your pipeline**

1. Choose **Activate**\.

1. In the confirmation dialog box, choose **Close**\.