# Copy Data to Amazon Redshift Using the Command Line<a name="dp-copydata-redshift-cli"></a>

This tutorial demonstrates how to copy data from Amazon S3 to Amazon Redshift\. You'll create a new table in Amazon Redshift, and then use AWS Data Pipeline to transfer data to this table from a public Amazon S3 bucket, which contains sample input data in CSV format\. The logs are saved to an Amazon S3 bucket that you own\.

Amazon S3 is a web service that enables you to store data in the cloud\. For more information, see the [Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)\. Amazon Redshift is a data warehouse service in the cloud\. For more information, see the [Amazon Redshift Cluster Management Guide](https://docs.aws.amazon.com/redshift/latest/mgmt/)\.

**Prerequisites**

Before you begin, you must complete the following steps:

1. Install and configure a command line interface \(CLI\)\. For more information, see [Accessing AWS Data Pipeline](what-is-datapipeline.md#accessing-datapipeline)\.

1. Ensure that the IAM roles named **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** exist\. The AWS Data Pipeline console creates these roles for you automatically\. If you haven't used the AWS Data Pipeline console at least once, then you must create these roles manually\. For more information, see [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\.

1. Set up the `COPY` command in Amazon Redshift, since you will need to have these same options working when you perform the copying within AWS Data Pipeline\. For information, see [Before You Begin: Configure COPY Options and Load Data](dp-learn-copy-redshift.md)\.

1. Set up an Amazon Redshift database\. For more information, see [Set up Pipeline, Create a Security Group, and Create an Amazon Redshift Cluster](dp-copydata-redshift-prereq.md)\.

**Topics**
+ [Define a Pipeline in JSON Format](dp-copydata-redshift-define-pipeline-cli.md)
+ [Upload and Activate the Pipeline Definition](dp-copydata-redshift-upload-cli.md)