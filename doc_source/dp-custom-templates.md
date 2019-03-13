# Creating a Pipeline Using Parametrized Templates<a name="dp-custom-templates"></a>

You can use a parametrized template to customize a pipeline definition\. This enables you to create a common pipeline definition but provide different parameters when you add the pipeline definition to a new pipeline\.

**Topics**
+ [Add myVariables to the Pipeline Definition](#add-pipeline-variables)
+ [Define Parameter Objects](#define-pipeline-parameter-objects)
+ [Define Parameter Values](#define-pipeline-parameter-values)
+ [Submitting the Pipeline Definition](#submit-pipeline-definition)

## Add myVariables to the Pipeline Definition<a name="add-pipeline-variables"></a>

When you create the pipeline definition file, specify variables using the following syntax: \#\{my*Variable*\}\. It is required that the variable is prefixed by `my`\. For example, the following pipeline definition file, `pipeline-definition.json`, includes the following variables: *myShellCmd*, *myS3InputLoc*, and *myS3OutputLoc*\.

**Note**  
A pipeline definition has an upper limit of 50 parameters\.

```
{ 
  "objects": [
    {
      "id": "ShellCommandActivityObj",
      "input": {
        "ref": "S3InputLocation"
      },
      "name": "ShellCommandActivityObj",
      "runsOn": {
        "ref": "EC2ResourceObj"
      },
      "command": "#{myShellCmd}",
      "output": {
        "ref": "S3OutputLocation"
      },
      "type": "ShellCommandActivity",
      "stage": "true"
    },
    {
      "id": "Default",
      "scheduleType": "CRON",
      "failureAndRerunMode": "CASCADE",
      "schedule": {
        "ref": "Schedule_15mins"
      },
      "name": "Default",
      "role": "DataPipelineDefaultRole",
      "resourceRole": "DataPipelineDefaultResourceRole"
    },
    {
      "id": "S3InputLocation",
      "name": "S3InputLocation",
      "directoryPath": "#{myS3InputLoc}",
      "type": "S3DataNode"
    },
    {
      "id": "S3OutputLocation",
      "name": "S3OutputLocation",
      "directoryPath": "#{myS3OutputLoc}/#{format(@scheduledStartTime, 'YYYY-MM-dd-HH-mm-ss')}",
      "type": "S3DataNode"
    },
    {
      "id": "Schedule_15mins",
      "occurrences": "4",
      "name": "Every 15 minutes",
      "startAt": "FIRST_ACTIVATION_DATE_TIME",
      "type": "Schedule",
      "period": "15 Minutes"
    },
    {
      "terminateAfter": "20 Minutes",
      "id": "EC2ResourceObj",
      "name": "EC2ResourceObj",
	  "instanceType":"t1.micro",
      "type": "Ec2Resource"
    }
  ]
}
```

## Define Parameter Objects<a name="define-pipeline-parameter-objects"></a>

You can create a separate file with parameter objects that defines the variables in your pipeline definition\. For example, the following JSON file, `parameters.json`, contains parameter objects for the *myShellCmd*, *myS3InputLoc*, and *myS3OutputLoc* variables from the example pipeline definition above\.

```
{
  "parameters": [
    {
      "id": "myShellCmd",
      "description": "Shell command to run",
      "type": "String",
      "default": "grep -rc \"GET\" ${INPUT1_STAGING_DIR}/* > ${OUTPUT1_STAGING_DIR}/output.txt"
    },
    {
      "id": "myS3InputLoc",
      "description": "S3 input location",
      "type": "AWS::S3::ObjectKey",
      "default": "s3://us-east-1.elasticmapreduce.samples/pig-apache-logs/data"
    },
    {
      "id": "myS3OutputLoc",
      "description": "S3 output location",
      "type": "AWS::S3::ObjectKey"
    }
  ]
}
```

**Note**  
You could add these objects directly to the pipeline definition file instead of using a separate file\.

The following table describes the attributes for parameter objects\.


**Parameter Attributes**  

| Attribute | Type | Description | 
| --- | --- | --- | 
| id | String | The unique identifier of the parameter\. To mask the value while it is typed or displayed, add an asterisk \('\*'\) as a prefix\. For example, \*myVariable—\. Notes that this also encrypts the value before it is stored by AWS Data Pipeline\. | 
| description | String | A description of the parameter\. | 
| type | String, Integer, Double, or AWS::S3::ObjectKey | The parameter type that defines the allowed range of input values and validation rules\. The default is String\. | 
| optional | Boolean | Indicates whether the parameter is optional or required\. The default is false\. | 
| allowedValues | List of Strings | Enumerates all permitted values for the parameter\. | 
| default | String | The default value for the parameter\. If you specify a value for this parameter using parameter values, it overrides the default value\. | 
| isArray | Boolean | Indicates whether the parameter is an array\. | 

## Define Parameter Values<a name="define-pipeline-parameter-values"></a>

You can create a separate file to define your variables using parameter values\. For example, the following JSON file, `file://values.json`, contains the value for *myS3OutputLoc* variable from the example pipeline definition above\.

```
{
  "values": 
    {
      "myS3OutputLoc": "myOutputLocation"
    }
}
```

## Submitting the Pipeline Definition<a name="submit-pipeline-definition"></a>

When you submit your pipeline definition, you can specify parameters, parameter objects, and parameter values\. For example, you can use the [put\-pipeline\-definition](https://docs.aws.amazon.com/cli/latest/reference/datapipeline/put-pipeline-definition.html) AWS CLI command as follows:

```
$ aws datapipeline put-pipeline-definition --pipeline-id id --pipeline-definition file://pipeline-definition.json \ 
--parameter-objects file://parameters.json --parameter-values-uri file://values.json
```

**Note**  
A pipeline definition has an upper limit of 50 parameters\. The size of the file for `parameter-values-uri` has an upper limit of 15 KB\.