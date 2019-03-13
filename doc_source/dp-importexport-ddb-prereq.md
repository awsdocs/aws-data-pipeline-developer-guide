# Before You Begin<a name="dp-importexport-ddb-prereq"></a>

Be sure to complete the following steps:
+ Complete the tasks in [Setting up for AWS Data Pipeline](dp-get-setup.md)\.
+ \(Optional\) Set up a VPC for the cluster and a security group for the VPC\. For more information, see [Launching Resources for Your Pipeline into a VPC](dp-resources-vpc.md)\.
+ Create a topic and subscribe to receive notifications from AWS Data Pipeline regarding the status of your pipeline components\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html) in the *Amazon Simple Notification Service Getting Started Guide*\.
+ Create a DynamoDB table to store data\. For more information, see [Create a DynamoDB Table](#dp-importexport-ddb-table-cli)\.

Be aware of the following:
+ Imports may overwrite data in your DynamoDB table\. When you import data from Amazon S3, the import may overwrite items in your DynamoDB table\. Make sure that you are importing the right data and into the right table\. Be careful not to accidentally set up a recurring import pipeline that will import the same data multiple times\.
+ Exports may overwrite data in your Amazon S3 bucket\. When you export data to Amazon S3, you may overwrite previous exports if you write to the same bucket path\. The default behavior of the **Export DynamoDB to S3** template will append the job's scheduled time to the Amazon S3 bucket path, which will help you avoid this problem\.
+ Import and Export jobs will consume some of your DynamoDB table's provisioned throughput capacity\. This section explains how to schedule an import or export job using Amazon EMR\. The Amazon EMR cluster will consume some read capacity during exports or write capacity during imports\. You can control the percentage of the provisioned capacity that the import/export jobs consume by with the settings M`yImportJob.myDynamoDBWriteThroughputRatio` and `MyExportJob.myDynamoDBReadThroughputRatio`\. Be aware that these settings determine how much capacity to consume at the beginning of the import/export process and will not adapt in real time if you change your table's provisioned capacity in the middle of the process\.
+ Be aware of the costs\. AWS Data Pipeline manages the import/export process for you, but you still pay for the underlying AWS services that are being used\. The import and export pipelines will create Amazon EMR clusters to read and write data and there are per\-instance charges for each node in the cluster\. You can read more about the details of [Amazon EMR Pricing](https://aws.amazon.com/elasticmapreduce/pricing/)\. The default cluster configuration is one m1\.small instance master node and one m1\.xlarge instance task node, though you can change this configuration in the pipeline definition\. There are also charges for AWS Data Pipeline\. For more information, see [AWS Data Pipeline Pricing](https://aws.amazon.com/datapipeline/pricing/) and [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/)\.
+ If a table is configured for On\-Demand Capacity, change the table back to provisioned capacity before running the export or import operations\. On\-Demand Capacity removes provisioned throughput and the pipeline will need a throughput ratio to calculate resources to use from the DynamoDB table\. You may use CloudWatch metrics to evaluate the aggregate of throughput the table has used and provision the throughput capacity accordingly\.

## Create a DynamoDB Table<a name="dp-importexport-ddb-table-cli"></a>

You can create the DynamoDB table that is required for this tutorial\. If you already have a DynamoDB table, you can skip this procedure to create one\.

For more information, see [Working with Tables in DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithDDTables.html) in the *Amazon DynamoDB Developer Guide*\.

**To create a DynamoDB table**

1. Open the DynamoDB console at [https://console\.aws\.amazon\.com/dynamodb/](https://console.aws.amazon.com/dynamodb/)\.

1. Click **Create Table**\.

1. Enter a unique name for your table in **Table Name**\.

1. In the **Primary Key: Partition Key** field, enter the number `Id`\.

1. Click **Continue** to skip the optional **Add Indexes** page\.

1. On the **Provisioned Throughput Capacity** page, do the following\. Note that these values are small because the sample data is small\. For information about calculating the required size for your own data, see [Provisioned Throughput in Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/ProvisionedThroughputIntro.html) in the *Amazon DynamoDB Developer Guide*\.

   1. In **Read Capacity Units**, enter `5`\.

   1. In **Write Capacity Units**, enter `5`\.

   1. Click **Continue**\.

1. On the **Throughput Alarms** page, in **Send notification to**, enter your email address, and then click **Continue**\.

1. On the **Review** page, click **Create**\.