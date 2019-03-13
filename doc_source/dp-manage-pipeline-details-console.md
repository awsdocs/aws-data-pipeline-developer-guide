# Viewing Pipeline Instance Details<a name="dp-manage-pipeline-details-console"></a>

You can monitor the progress of your pipeline\. For more information about instance status, see [Interpreting Pipeline Status Details](dp-pipeline-status.md)\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.

**To monitor the progress of a pipeline using the console**

1. On the **List Pipelines** page, in the **Pipeline ID** column, click the arrow for your pipeline and click **View execution details**\.

1. The **Execution details** page lists the name, type, status, and schedule information of each component\.

   You can then click on the arrow for each component name to view dependency information for that component\.   
![\[Instance summary pane\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-component-dropdown.png)

   In the inline summary, you can view instance details, re\-run an activity, mark it as `FINISHED`, or explore the dependency chain\.   
![\[Instance summary pane\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-component-dependency-chain.png)
**Note**  
If you do not see runs listed, check when your pipeline was scheduled\. Either change **End \(in UTC\)** to a later date or change **Start \(in UTC\)** an earlier date, and then click **Update**\.

1. If the **Status** column of all components in your pipeline is `FINISHED`, your pipeline has successfully completed the activity\. You should receive an email about the successful completion of this task, to the account that you specified to receive Amazon SNS notifications\.

   You can also check the content of your output data node\.

1. If the **Status** column of any component in your pipeline is not `FINISHED`, either your pipeline is waiting for some dependency or it has failed\. To troubleshoot failed or the incomplete instance runs, use the following procedure\.

1. Click the triangle next to a component or activity\.

   If the status of the instance is `FAILED`, the **Attempts** box has an **Error Message** indicating the reason for failure under the latest attempt\. For example, `Status Code: 403, AWS Service: Amazon S3, AWS Request ID: 1A3456789ABCD, AWS Error Code: null, AWS Error Message: Forbidden`\. You can also click on **More\.\.\.** in the **Details** column to view the instance details of this attempt\.

1. To take action on your incomplete or failed component, choose **Rerun**, **Mark Finished**, or **Cancel**\.

**To monitor the progress of a pipeline using the AWS CLI**  
To retrieve pipeline instance details, such as a history of the times that a pipeline has run, use the [list\-runs](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/list-runs.html) command\. This command enables you to filter the list of runs returned based on either their current status or the date\-range in which they were launched\. Filtering the results is useful because, depending on the pipeline's age and scheduling, the run history can be large\.

The following example retrieves information for all runs\.

```
aws datapipeline list-runs --pipeline-id df-00627471SOVYZEXAMPLE
```

The following example retrieves information for all runs that have completed\.

```
aws datapipeline list-runs --pipeline-id df-00627471SOVYZEXAMPLE --status finished
```

The following example retrieves information for all runs launched in the specified time frame\.

```
aws datapipeline list-runs --pipeline-id df-00627471SOVYZEXAMPLE --start-interval "2013-09-02","2013-09-11"
```