# Identifying the Amazon EMR Cluster that Serves Your Pipeline<a name="dp-troubleshoot-emr"></a>

If an `EMRCluster` or `EMRActivity` fails and the error information provided by the AWS Data Pipeline console is unclear, you can identify the Amazon EMR cluster that serves your pipeline using the Amazon EMR console\. This helps you locate the logs that Amazon EMR provides to get more details about errors that occur\.

**To see more detailed Amazon EMR error information**

1. In the AWS Data Pipeline console, select the triangle next to the pipeline instance, to expand the instance details\. 

1. Choose **View execution details** and select the triangle next to the component\.

1. In the **Details** column, choose **More\.\.\.**\. The information screen opens listing the details of the component\. Locate and copy the **instanceParent** value from the screen, such as: `@EmrActivityId_xiFDD_2017-09-30T21:40:13` 

1. Navigate to the Amazon EMR console, search for a cluster with the matching **instanceParent** value in its name, and then choose **Debug**\. 
**Note**  
For the **Debug** button to function, your pipeline definition must have set the EmrActivity `enableDebugging` option to `true` and the `EmrLogUri` option to a valid path\.

1. Now that you know which Amazon EMR cluster contains the error that causes your pipeline failure, follow the [Troubleshooting Tips](https://docs.aws.amazon.com/ElasticMapReduce/latest/DeveloperGuide/Debugging.html) in the *Amazon EMR Developer Guide*\.