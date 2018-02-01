# Viewing Pipeline Logs<a name="dp-viewing-logs"></a>

Pipeline\-level logging is supported at pipeline creation by specifying an Amazon S3 location in either the console or with a `pipelineLogUri` in the default object in SDK/CLI\. The directory structure for each pipeline within that URI is like the following:

```
pipelineId
    -componentName
        -instanceId
            -attemptId
```

For pipeline, `df-00123456ABC7DEF8HIJK`, the directory structure looks like:

```
df-00123456ABC7DEF8HIJK
    -ActivityId_fXNzc
        -@ActivityId_fXNzc_2014-05-01T00:00:00
            -@ActivityId_fXNzc_2014-05-01T00:00:00_Attempt=1
```

For `ShellCommandActivity`, logs for `stderr` and `stdout` associated with these activities are stored in the directory for each attempt\.

For resources like, `EmrCluster`, where an `emrLogUri` is set, that value takes precedence\. Otherwise, resources \(including TaskRunner logs for those resources\) follow the above pipeline logging structure\. You may view these logs for each component in the **Execution Details** page for your pipeline by viewing a component's details and clicking on the link for logs:

![\[Instance summary pane\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-logging-configured.png)

You can also view logs for each attempt\. For example, to view logs for a `HadoopActivity`, you can click the pipeline **Attempts** tab for your activity\. Hadoop Logs gives the logs created by Hadoop jobs\. 

![\[Instance summary pane\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-logs-hadoopactivity.png)