# Editing Your Pipeline<a name="dp-manage-pipeline-modify-console"></a>

To change some aspect of one of your pipelines, you can update its pipeline definition\. After you change a pipeline that is running, you must re\-activate the pipeline for your changes to take effect\. In addition, you can re\-run one or more pipeline components\.

**Topics**
+ [Limitations](#dp-edit-pipeline-limits)
+ [Editing a Pipeline Using the Console](#dp-edit-pipeline-console)
+ [Editing a Pipeline Using the AWS CLI](#dp-edit-pipeline-aws-cli)

## Limitations<a name="dp-edit-pipeline-limits"></a>

While the pipeline is in the `PENDING` state and is not activated, you can make any changes to it\. After you activate a pipeline, you can edit the pipeline with the following restrictions\. The changes you make apply to new runs of the pipeline objects after you save them and then activate the pipeline again\.
+ You can't remove an object
+ You can't change the schedule period of an existing object
+ You can't add, delete, or modify reference fields in an existing object
+ You can't reference an existing object in an output field of a new object
+ You can't change the scheduled start date of an object \(instead, activate the pipeline with a specific date and time\)

## Editing a Pipeline Using the Console<a name="dp-edit-pipeline-console"></a>

You can edit a pipeline using the AWS Management Console\.

**To edit a pipeline using the console**

1. On the **List Pipelines** page, check the **Pipeline ID** and **Name** columns for your pipeline, and then click your Pipeline ID\.

1. To complete or modify your pipeline definition:

   1. On the pipeline \(Architect\) page, click the object panes in the right pane and finish defining the objects and fields of your pipeline definition\. If you are modifying an active pipeline, some fields are grayed out and can't be modified\. It might be easier to clone the pipeline and edit the copy, depending on the changes you need to make\. For more information, see [Cloning Your Pipeline](dp-manage-pipeline-clone-console.md)\.

   1. Click **Save pipeline**\. If there are validation errors, fix them and save the pipeline again\.

1. After you've saved your pipeline definition with no validation errors, click **Activate**\.

1. In the **List Pipelines** page, check whether your newly created pipeline is listed and the **Schedule State** column displays `SCHEDULED`\.

1. After editing an active pipeline, you might decide to rerun one or more pipeline components\.

   On the **List Pipelines** page, in the detail dropdown of your pipeline, click **View execution details**\.

   1. On the **Execution details** page, choose a pipeline component dropdown from the list to view the details for a component\.

   1. Click **Rerun**\.

   1. At the confirmation prompt, click **Continue**\.

      The changed pipeline component and any dependencies change status\. For example, resources change to the `CREATING` status and activities change to the `WAITING_FOR_RUNNER` status\.

## Editing a Pipeline Using the AWS CLI<a name="dp-edit-pipeline-aws-cli"></a>

You can edit a pipeline using the command line tools\.

First, download a copy of the current pipeline definition using the [get\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/get-pipeline-definition.html) command\. By doing this, you can be sure that you are modifying the most recent pipeline definition\. The following example uses prints the pipeline definition to standard output \(stdout\)\.

```
aws datapipeline get-pipeline-definition --pipeline-id df-00627471SOVYZEXAMPLE
```

Save the pipeline definition to a file and edit it as needed\. Update your pipeline definition using the [put\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/put-pipeline-definition.html) command\. The following example uploads the updated pipeline definition file\.

```
aws datapipeline put-pipeline-definition --pipeline-id df-00627471SOVYZEXAMPLE --pipeline-definition file://MyEmrPipelineDefinition.json
```

You can retrieve the pipeline definition again using the `get-pipeline-definition` command to ensure that the update was successful\. To activate the pipeline, use the following [activate\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/activate-pipeline.html) command:

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```

If you prefer, you can activate the pipeline from a specific date and time, using the `--start-timestamp` option as follows:

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE --start-timestamp YYYY-MM-DDTHH:MM:SSZ
```

To re\-run one or more pipeline components, use the [set\-status](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/set-status.html) command\.