# Copy CSV Data Using the Command Line<a name="dp-get-started-copy-data-cli"></a>

You can create and use pipelines to copy data from one Amazon S3 bucket to another\.

**Prerequisites**

Before you begin, you must complete the following steps:

1. Install and configure a command line interface \(CLI\)\. For more information, see [Accessing AWS Data Pipeline](what-is-datapipeline.md#accessing-datapipeline)\.

1. Ensure that the IAM roles named **DataPipelineDefaultRole** and **DataPipelineDefaultResourceRole** exist\. The AWS Data Pipeline console creates these roles for you automatically\. If you haven't used the AWS Data Pipeline console at least once, then you must create these roles manually\. For more information, see [IAM Roles for AWS Data Pipeline](dp-iam-roles.md)\.

**Topics**
+ [Define a Pipeline in JSON Format](#dp-copy-define-pipeline-cli)
+ [Upload and Activate the Pipeline Definition](#dp-copy-json-upload-cli)

## Define a Pipeline in JSON Format<a name="dp-copy-define-pipeline-cli"></a>

This example scenario shows how to use JSON pipeline definitions and the AWS Data Pipeline CLI to schedule copying data between two Amazon S3 buckets at a specific time interval\. This is the full pipeline definition JSON file followed by an explanation for each of its sections\. 

**Note**  
 We recommend that you use a text editor that can help you verify the syntax of JSON\-formatted files, and name the file using the \.json file extension\.

In this example, for clarity, we skip the optional fields and show only required fields\. The complete pipeline JSON file for this example is:

```
{
  "objects": [
    {
      "id": "MySchedule",
      "type": "Schedule",
      "startDateTime": "2013-08-18T00:00:00",
      "endDateTime": "2013-08-19T00:00:00",
      "period": "1 day"
    },
    {
      "id": "S3Input",
      "type": "S3DataNode",
      "schedule": {
        "ref": "MySchedule"
      },
      "filePath": "s3://example-bucket/source/inputfile.csv"
    },
    {
      "id": "S3Output",
      "type": "S3DataNode",
      "schedule": {
        "ref": "MySchedule"
      },
      "filePath": "s3://example-bucket/destination/outputfile.csv"
    },
    {
      "id": "MyEC2Resource",
      "type": "Ec2Resource",
      "schedule": {
        "ref": "MySchedule"
      },
      "instanceType": "m1.medium",
      "role": "DataPipelineDefaultRole",
      "resourceRole": "DataPipelineDefaultResourceRole"
    },
    {
      "id": "MyCopyActivity",
      "type": "CopyActivity",
      "runsOn": {
        "ref": "MyEC2Resource"
      },
      "input": {
        "ref": "S3Input"
      },
      "output": {
        "ref": "S3Output"
      },
      "schedule": {
        "ref": "MySchedule"
      }
    }
  ]
}
```

### Schedule<a name="dp-copy-json-schedule-cli"></a>

The pipeline defines a schedule with a begin and end date, along with a period to determine how frequently the activity in this pipeline runs\. 

```
{
  "id": "MySchedule",
  "type": "Schedule",
  "startDateTime": "2013-08-18T00:00:00",
  "endDateTime": "2013-08-19T00:00:00",
  "period": "1 day"
},
```

### Amazon S3 Data Nodes<a name="dp-copy-json-s3-node-cli"></a>

 Next, the input S3DataNode pipeline component defines a location for the input files; in this case, an Amazon S3 bucket location\. The input S3DataNode component is defined by the following fields: 

```
{
  "id": "S3Input",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "filePath": "s3://example-bucket/source/inputfile.csv"
},
```

Id  
The user\-defined name for the input location \(a label for your reference only\)\.

Type  
The pipeline component type, which is "S3DataNode" to match the location where the data resides, in an Amazon S3 bucket\.

Schedule  
A reference to the schedule component that we created in the preceding lines of the JSON file labeled “MySchedule”\.

Path  
The path to the data associated with the data node\. The syntax for a data node is determined by its type\. For example, the syntax for an Amazon S3 path follows a different syntax that is appropriate for a database table\.

 Next, the output S3DataNode component defines the output destination location for the data\. It follows the same format as the input S3DataNode component, except the name of the component and a different path to indicate the target file\. 

```
{
  "id": "S3Output",
  "type": "S3DataNode",
  "schedule": {
    "ref": "MySchedule"
  },
  "filePath": "s3://example-bucket/destination/outputfile.csv"
},
```

### Resource<a name="dp-copy-json-resource-cli"></a>

This is a definition of the computational resource that performs the copy operation\. In this example, AWS Data Pipeline should automatically create an EC2 instance to perform the copy task and terminate the resource after the task completes\. The fields defined here control the creation and function of the EC2 instance that does the work\. The EC2Resource is defined by the following fields: 

```
{
  "id": "MyEC2Resource",
  "type": "Ec2Resource",
  "schedule": {
    "ref": "MySchedule"
  },
  "instanceType": "m1.medium",
  "role": "DataPipelineDefaultRole",
  "resourceRole": "DataPipelineDefaultResourceRole"
},
```

Id  
The user\-defined name for the pipeline schedule, which is a label for your reference only\.

Type  
The type of computational resource to perform work; in this case, an EC2 instance\. There are other resource types available, such as an EmrCluster type\.

Schedule  
The schedule on which to create this computational resource\.

instanceType  
The size of the EC2 instance to create\. Ensure that you set the appropriate size of EC2 instance that best matches the load of the work that you want to perform with AWS Data Pipeline\. In this case, we set an m1\.medium EC2 instance\. For more information about the different instance types and when to use each one, see [Amazon EC2 Instance Types](http://aws.amazon.com/ec2/instance-types/) topic at http://aws\.amazon\.com/ec2/instance\-types/\.

Role  
The IAM role of the account that accesses resources, such as accessing an Amazon S3 bucket to retrieve data\.

resourceRole  
The IAM role of the account that creates resources, such as creating and configuring an EC2 instance on your behalf\. Role and ResourceRole can be the same role, but separately provide greater granularity in your security configuration\.

### Activity<a name="dp-copy-json-activity-cli"></a>

The last section in the JSON file is the definition of the activity that represents the work to perform\. This example uses `CopyActivity` to copy data from a CSV file in an http://aws\.amazon\.com/ec2/instance\-types/ bucket to another\. The `CopyActivity` component is defined by the following fields: 

```
{
  "id": "MyCopyActivity",
  "type": "CopyActivity",
  "runsOn": {
    "ref": "MyEC2Resource"
  },
  "input": {
    "ref": "S3Input"
  },
  "output": {
    "ref": "S3Output"
  },
  "schedule": {
    "ref": "MySchedule"
  }
}
```

Id  
The user\-defined name for the activity, which is a label for your reference only\.

Type  
The type of activity to perform, such as MyCopyActivity\.

runsOn  
The computational resource that performs the work that this activity defines\. In this example, we provide a reference to the EC2 instance defined previously\. Using the `runsOn` field causes AWS Data Pipeline to create the EC2 instance for you\. The `runsOn` field indicates that the resource exists in the AWS infrastructure, while the `workerGroup` value indicates that you want to use your own on\-premises resources to perform the work\.

Input  
The location of the data to copy\.

Output  
The target location data\.

Schedule  
The schedule on which to run this activity\.

## Upload and Activate the Pipeline Definition<a name="dp-copy-json-upload-cli"></a>

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