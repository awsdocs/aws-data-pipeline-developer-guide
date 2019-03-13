# Viewing Your Pipelines<a name="dp-list-pipelines"></a>

You can view your pipelines using the console or the command line interface \(CLI\)\.

**To view your pipelines using the console**

1. Open the AWS Data Pipeline console\. If you have created any pipelines in that region, the console displays them in a list\. Otherwise, you see a welcome screen\.  
![\[List\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-list-pipelines-page.png)

1. To view information about a pipeline, click the arrow\. The console displays information about the schedule, activities, and tags for the pipeline\. For more information about the health status, see [Interpreting Pipeline and Component Health State](dp-interpret-health-status.md)\.  
![\[List\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-list-pipelines-dropdown.png)

**To view your pipelines using the AWS CLI**
+ Use the following [list\-pipelines](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/list-pipelines.html) command to list your pipelines:

  ```
  aws datapipeline list-pipelines
  ```