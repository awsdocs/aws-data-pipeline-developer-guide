# Creating Pipelines Using Console Templates<a name="dp-console-templates"></a>

 The AWS Data Pipeline console provides several pre\-configured pipeline definitions, known as templates\. You can use templates to get started with AWS Data Pipeline quickly\. You can also create templates with parametrized values\. This allows you to specify pipeline objects with parameters and pre\-defined attributes\. You can then use a tool to create values for a specific purpose within the pipeline\. This allows you to reuse pipeline definitions with different values\. For more information, see [Creating a Pipeline Using Parametrized Templates](dp-custom-templates.md)\.

## Initialize, Create, and Schedule a Pipeline<a name="dp-create-pipeline"></a>

The AWS Data Pipeline console **Create Pipeline** page allows you to create and schedule a pipeline easily\.

**To create and schedule a pipeline**

1. Open the AWS Data Pipeline console at [https://console\.aws\.amazon\.com/datapipeline/](https://console.aws.amazon.com/datapipeline/)\.

1. Click either **Get started now** or **Create Pipeline**\.

1. Enter a pipeline name and an optional description for the pipeline\.

1. Choose **Build using Architect** to interactively create and edit nodes in a pipeline definition or **Build using a template** to select a template\. For more information about templates, see [Choose a Template](#dp-choose-templates)\.

   If you use choose to use a template, the console displays a form that is specific to that template under **Parameters**\. Complete the form as appropriate\.

1. Choose whether to run the pipeline once on activation or on a schedule\.

   If you choose to run the pipeline on a schedule:

   1. <a name="step_lhz_vpf_vn"></a>For **Run every**, choose a period for the pipeline\. The start and end time must define an interval that's long enough to accommodate this period\.

   1. Choose a **Starting** time\. If you choose **on pipeline activation**, the pipeline uses the current activation time\.

   1. Choose an **Ending** time\. If you choose **never**, the pipeline runs indefinitely\.

1. Select an option for **IAM Roles**\. If you select **Default**, AWS Data Pipeline assigns its own default roles\. You can optionally select **Custom** to choose other roles available to your account\. 

1. Click either **Edit in Architect** or **Activate**\.

## Choose a Template<a name="dp-choose-templates"></a>

When you choose a template, the pipeline create page populates with the parameters specified in the pipeline definition, such as custom Amazon S3 directory paths, Amazon EC2 key pair names, database connection strings, and so on\. You can provide this information at pipeline creation and activation\. The following templates available in the console are also available for download from the Amazon S3 bucket: `s3://datapipeline-us-east-1/templates/`\.

**Templates**
+ [Getting Started Using ShellCommandActivity](dp-template-gettingstartedshell.md)
+ [Run AWS CLI Command](dp-template-runawscli.md)
+ [Export DynamoDB Table to S3](dp-template-exportddbtos3.md)
+ [Import DynamoDB Backup Data from S3](dp-template-exports3toddb.md)
+ [Run Job on an Amazon EMR Cluster](dp-template-emr.md)
+ [Full Copy of Amazon RDS MySQL Table to Amazon S3](dp-template-copyrdstos3.md)
+ [Incremental Copy of Amazon RDS MySQL Table to Amazon S3](dp-template-incrementalcopyrdstos3.md) 
+ [Load S3 Data into Amazon RDS MySQL Table](dp-template-copys3tords.md)
+ [Full Copy of Amazon RDS MySQL Table to Amazon Redshift ](dp-template-redshiftrdsfull.md)
+ [Incremental Copy of an Amazon RDS MySQL Table to Amazon Redshift](dp-template-redshiftrdsincremental.md)
+ [Load Data from Amazon S3 into Amazon Redshift](dp-template-s3redshift.md)