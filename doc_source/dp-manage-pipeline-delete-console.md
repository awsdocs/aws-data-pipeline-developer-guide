# Deleting Your Pipeline<a name="dp-manage-pipeline-delete-console"></a>

When you no longer require a pipeline, such as a pipeline created during application testing, you should delete it to remove it from active use\. Deleting a pipeline puts it into a deleting state\. When the pipeline is in the deleted state, its pipeline definition and run history are gone\. Therefore, you can no longer perform operations on the pipeline, including describing it\.

**Important**  
You can't restore a pipeline after you delete it, so be sure that you won't need the pipeline in the future before you delete it\.

**To delete a pipeline using the console**

1. In the **List Pipelines** page, select the pipeline\.

1. Click **Actions**, and then click **Delete**\.

1. When prompted for confirmation, click **Delete**\.

**To delete a pipeline using the AWS CLI**  
To delete a pipeline, use the [delete\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/delete-pipeline.html) command\. The following command deletes the specified pipeline\.

```
aws datapipeline delete-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```