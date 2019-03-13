# Viewing Your Pipeline Definitions<a name="dp-view-definition"></a>

Use the AWS Data Pipeline console or the command line interface \(CLI\) to view your pipeline definition\. The console shows a graphical representation, while the CLI prints a pipeline definition file, in JSON format\. For information about the syntax and usage of pipeline definition files, see [Pipeline Definition File Syntax](dp-writing-pipeline-definition.md)\.

**To view a pipeline definition using the console**

1. On the **List Pipelines** page, click the **Pipeline ID** for the desired pipeline, which displays the pipeline **Architect** page\.

1. On the pipeline **Architect** page, click the object icons in the design pane to expand the corresponding section in the right pane\.

   Alternatively, expand one of the sections in the right pane to view its objects and their associated fields\.

1. If your pipeline definition graph does not fit in the design pane, use the pan buttons on the right side of the design pane to slide the canvas\.  
![\[Pan buttons\]](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/images/dp-pan-buttons.png)

1. You can also view the entire text pipeline definition by clicking **Export**\. A dialog appears with the JSON pipeline definition\. 

If you are using the CLI, it's a good idea to retrieve the pipeline definition before you submit modifications, because it's possible that another user or process changed the pipeline definition after you last worked with it\. By downloading a copy of the current definition and using that as the basis for your modifications, you can be sure that you are working with the most recent pipeline definition\. It's also a good idea to retrieve the pipeline definition again after you modify it, so that you can ensure that the update was successful\.

If you are using the CLI, you can get two different versions of your pipeline\. The `active` version is the pipeline that is currently running\. The `latest` version is a copy that's created when you edit a running pipeline\. When you upload the edited pipeline, it becomes the `active` version and the previous `active` version is no longer available\.

**To get a pipeline definition using the AWS CLI**  
To get the complete pipeline definition, use the [get\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/get-pipeline-definition.html) command\. The pipeline definition is printed to standard output \(stdout\)\.

The following example gets the pipeline definition for the specified pipeline\.

```
aws datapipeline get-pipeline-definition --pipeline-id df-00627471SOVYZEXAMPLE
```

To retrieve a specific version of a pipeline, use the `--version` option\. The following example retrieves the `active` version of the specified pipeline\.

```
aws datapipeline get-pipeline-definition --version active --id df-00627471SOVYZEXAMPLE
```