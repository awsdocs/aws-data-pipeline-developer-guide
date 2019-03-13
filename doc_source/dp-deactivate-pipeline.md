# Deactivating Your Pipeline<a name="dp-deactivate-pipeline"></a>

Deactivating a running pipeline pauses the pipeline execution\. To resume pipeline execution, you can activate the pipeline\. This enables you to make changes\. For example, if you are writing data to a database that is scheduled to undergo maintenance, you can deactivate the pipeline, wait for the maintenance to complete, and then activate the pipeline\.

When you deactivate a pipeline, you can specify what happens to running activities\. By default, these activities are canceled immediately\. Alternatively, you can have AWS Data Pipeline wait until the activities finish before deactivating the pipeline\.

When you activate a deactivated pipeline, you can specify when it resumes\. For example, using the AWS Management Console, you can resume after the last completed run, from the current time, or from a specified date and time\. Using the AWS CLI or the API, the pipeline resumes from the last completed execution by default, or you can specify the date and time to resume the pipeline\.

**Topics**
+ [Deactivate Your Pipeline Using the Console](#dp-deactivate-pipeline-console)
+ [Deactivate Your Pipeline Using the AWS CLI](#dp-deactivate-pipeline-cli)

## Deactivate Your Pipeline Using the Console<a name="dp-deactivate-pipeline-console"></a>

Use the following procedure to deactivate a running pipeline\.

**To deactivate a pipeline**

1. In the **List Pipelines** page, select the pipeline to deactivate\.

1. Click **Actions**, and then click **Deactivate**\.

1. In the **Deactivate a Pipeline** dialog box, select an option, and then click **Deactivate**\.

1. When prompted for confirmation, click **Deactivate**\.

When you are ready to resume the pipeline runs, use the following procedure to activate the deactivated pipeline\.

**To activate a pipeline**

1. In the **List Pipelines** page, select the pipeline to activate\.

1. Click **Actions**, and then click **Activate**\.

1. In the **Activate a Pipeline** dialog box, select an option, and then choose **Activate**\.

## Deactivate Your Pipeline Using the AWS CLI<a name="dp-deactivate-pipeline-cli"></a>

Use the following [deactivate\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/deactivate-pipeline.html) command to deactivate a pipeline:

```
aws datapipeline deactivate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```

To deactivate the pipeline only after all running activities finish, add the `--no-cancel-active` option, as follows:

```
aws datapipeline deactivate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE --no-cancel-active
```

When you are ready, you can resume the pipeline execution where it left off using the following [activate\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/activate-pipeline.html) command:

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```

To start the pipeline from a specific date and time, add the `--start-timestamp` option, as follows:

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE --start-timestamp YYYY-MM-DDTHH:MM:SSZ
```