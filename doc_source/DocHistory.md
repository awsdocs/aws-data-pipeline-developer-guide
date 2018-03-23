# Document History<a name="DocHistory"></a>

This documentation is associated with the 2012\-10\-29 version of AWS Data Pipeline\.

 **Latest documentation update: 22 March 2018\.** 


| Change | Description | Release Date | 
| --- | --- | --- | 
| Added a list of supported Amazon EC2 and Amazon EMR instances\. | Added a list of instances that AWS Data Pipeline creates by default, if you do not specify an instance type in the pipeline definition\. Added a list of supported Amazon EC2 and Amazon EMR instances\. For more information, see [Supported Instance Types for Pipeline Work Activities](dp-supported-instance-types.md)\. | 22 March 2018 | 
| Add support for On\-demand pipelines |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/DocHistory.html)  | 22 February 2016 | 
| Additional support for RDS databases |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/DocHistory.html)  | 17 August 2015 | 
| Additional JDBC support |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/DocHistory.html)  | 7 July 2015 | 
| HadoopActivity, Availability Zone, and Spot Support |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/DocHistory.html)  | 1 June 2015 | 
| Deactivating pipelines |  Added support for deactivating active pipelines\. For more information, see [Deactivating Your Pipeline](dp-deactivate-pipeline.md)\.  | 7 April 2015 | 
| Updated templates and console |  Added new templates as reflected in the console\. Updated the Getting Started chapter to use the **Getting Started with ShellCommandActivity** template\. For more information, see [Creating Pipelines Using Console Templates](dp-console-templates.md)\.  | 25 November 2014 | 
| VPC support |  Added support for launching resources into a virtual private cloud \(VPC\)\. For more information, see [Launching Resources for Your Pipeline into a VPC](dp-resources-vpc.md)\.  | 12 March 2014 | 
| Region support |  Added support for multiple service regions\. In addition to `us-east-1`, AWS Data Pipeline is supported in `eu-west-1`, `ap-northeast-1`, `ap-southeast-2`, and `us-west-2`\.  | 20 February 2014 | 
| Amazon Redshift support |  Added support for Amazon Redshift in AWS Data Pipeline, including a new console template \(Copy to Redshift\) and a tutorial to demonstrate the template\. For more information, see [Copy Data to Amazon Redshift Using AWS Data Pipeline](dp-copydata-redshift.md), [RedshiftDataNode](dp-object-redshiftdatanode.md), [RedshiftDatabase](dp-object-redshiftdatabase.md), and [RedshiftCopyActivity](dp-object-redshiftcopyactivity.md)\.  | 6 November 2013 | 
| PigActivity |  Added PigActivity, which provides native support for Pig\. For more information, see [PigActivity](dp-object-pigactivity.md)\.  | 15 October 2013 | 
| New console template, activity, and data format |  Added the new CrossRegion DynamoDB Copy console template, including the new HiveCopyActivity and DynamoDBExportDataFormat\.  | 21 August 2013 | 
| Cascading failures and reruns |  Added information about AWS Data Pipeline cascading failure and rerun behavior\. For more information, see [Cascading Failures and Reruns](dp-manage-cascade-failandrerun.md)\.   | 8 August 2013 | 
| Troubleshooting video |  Added the AWS Data Pipeline Basic Troubleshooting video\. For more information, see [Troubleshooting](dp-troubleshooting.md)\.   | 17 July 2013 | 
| Editing active pipelines |  Added more information about editing active pipelines and rerunning pipeline components\. For more information, see [Editing Your Pipeline](dp-manage-pipeline-modify-console.md)\.   | 17 July 2013 | 
| Use resources in different regions |  Added more information about using resources in different regions\. For more information, see [Using a Pipeline with Resources in Multiple Regions](dp-manage-region.md)\.   | 17 June 2013 | 
| WAITING\_ON\_DEPENDENCIES status |  CHECKING\_PRECONDITIONS status changed to WAITING\_ON\_DEPENDENCIES and added the @waitingOn runtime field for pipeline objects\.  | 20 May 2013 | 
| DynamoDBDataFormat |  Added DynamoDBDataFormat template\.  | 23 April 2013 | 
| Process Web Logs video and Spot Instances support |  Introduced the video "Process Web Logs with AWS Data Pipeline, Amazon EMR, and Hive," and Amazon EC2 Spot Instances support\.  | 21 February 2013 | 
|  |  The initial release of the AWS Data Pipeline Developer Guide\.   | 20 December 2012 | 