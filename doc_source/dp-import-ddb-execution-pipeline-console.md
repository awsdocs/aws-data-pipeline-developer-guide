# Step 4: Monitor the Pipeline Runs<a name="dp-import-ddb-execution-pipeline-console"></a>

After you activate your pipeline, you are taken to the **Execution details** page where you can monitor the progress of your pipeline\.

**To monitor the progress of your pipeline runs**

1. Choose **Update** or press F5 to update the status displayed\.
**Tip**  
If there are no runs listed, ensure that **Start \(in UTC\)** and **End \(in UTC\)** cover the scheduled start and end of your pipeline, and then choose **Update**\.

1. When the status of every object in your pipeline is `FINISHED`, your pipeline has successfully completed the scheduled tasks\. If you created an SNS notification, you should receive email about the successful completion of this task\.

1. If your pipeline doesn't complete successfully, check your pipeline settings for issues\. For more information about troubleshooting failed or incomplete instance runs of your pipeline, see [Resolving Common Problems](dp-check-when-run-fails.md)\.