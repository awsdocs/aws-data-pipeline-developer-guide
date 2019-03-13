# Upload and Activate the Pipeline Definition<a name="dp-copydata-redshift-upload-cli"></a>

You must upload your pipeline definition and activate your pipeline\. In the following example commands, replace *pipeline\_name* with a label for your pipeline and *pipeline\_file* with the fully\-qualified path for the pipeline definition `.json` file\.

**AWS CLI**

To create your pipeline definition and activate your pipeline, use the following [create\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/create-pipeline.html) command\. Note the ID of your pipeline, because you'll use this value with most CLI commands\.

```
aws datapipeline create-pipeline --name pipeline_name --unique-id token
{
    "pipelineId": "df-00627471SOVYZEXAMPLE"
}
```

To upload your pipeline definition, use the following [put\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/put-pipeline-definition.html) command\.

```
aws datapipeline put-pipeline-definition --pipeline-id df-00627471SOVYZEXAMPLE --pipeline-definition file://MyEmrPipelineDefinition.json
```

If you pipeline validates successfully, the `validationErrors` field is empty\. You should review any warnings\.

To activate your pipeline, use the following [activate\-pipeline](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/activate-pipeline.html) command\.

```
aws datapipeline activate-pipeline --pipeline-id df-00627471SOVYZEXAMPLE
```

You can verify that your pipeline appears in the pipeline list using the following [list\-pipelines](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/list-pipelines.html) command\.

```
aws datapipeline list-pipelines
```