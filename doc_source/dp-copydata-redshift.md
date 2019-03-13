# Copy Data to Amazon Redshift Using AWS Data Pipeline<a name="dp-copydata-redshift"></a>

This tutorial walks you through the process of creating a pipeline that periodically moves data from Amazon S3 to Amazon Redshift using either the **Copy to Redshift** template in the AWS Data Pipeline console, or a pipeline definition file with the AWS Data Pipeline CLI\.

Amazon S3 is a web service that enables you to store data in the cloud\. For more information, see the [Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)\. 

Amazon Redshift is a data warehouse service in the cloud\. For more information, see the [Amazon Redshift Cluster Management Guide](https://docs.aws.amazon.com/redshift/latest/mgmt/)\.

This tutorial has several prerequisites\. After completing the following steps, you can continue the tutorial using either the console or the CLI\.

**Topics**
+ [Before You Begin: Configure COPY Options and Load Data](dp-learn-copy-redshift.md)
+ [Set up Pipeline, Create a Security Group, and Create an Amazon Redshift Cluster](dp-copydata-redshift-prereq.md)
+ [Copy Data to Amazon Redshift Using the AWS Data Pipeline Console](dp-copydata-redshift-create.md)
+ [Copy Data to Amazon Redshift Using the Command Line](dp-copydata-redshift-cli.md)