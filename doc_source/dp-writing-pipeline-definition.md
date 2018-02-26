# Pipeline Definition File Syntax<a name="dp-writing-pipeline-definition"></a>

The instructions in this section are for working manually with pipeline definition files using the AWS Data Pipeline command line interface \(CLI\)\. This is an alternative to designing a pipeline interactively using the AWS Data Pipeline console\.

You can manually create pipeline definition files using any text editor that supports saving files using the UTF\-8 file format, and submit the files using the AWS Data Pipeline command line interface\. 

AWS Data Pipeline also supports a variety of complex expressions and functions within pipeline definitions\. For more information, see [Pipeline Expressions and Functions](dp-expressions-functions.md)\. 

## File Structure<a name="dp-file-structure"></a>

The first step in pipeline creation is to compose pipeline definition objects in a pipeline definition file\. The following example illustrates the general structure of a pipeline definition file\. This file defines two objects, which are delimited by '\{' and '\}', and separated by a comma\.

In the following example, the first object defines two name\-value pairs, known as *fields*\. The second object defines three fields\.

```
{
  "objects" : [
    {
       "name1" : "value1",
       "name2" : "value2"
    },
    {
       "name1" : "value3",
       "name3" : "value4",
       "name4" : "value5"
    }
  ]
}
```

When creating a pipeline definition file, you must select the types of pipeline objects that you need, add them to the pipeline definition file, and then add the appropriate fields\. For more information about pipeline objects, see [Pipeline Object Reference](dp-pipeline-objects.md)\.

For example, you could create a pipeline definition object for an input data node and another for the output data node\. Then create another pipeline definition object for an activity, such as processing the input data using Amazon EMR\. 

## Pipeline Fields<a name="dp-add-fields"></a>

After you know which object types to include in your pipeline definition file, you add fields to the definition of each pipeline object\. Field names are enclosed in quotes, and are separated from field values by a space, a colon, and a space, as shown in the following example\.

```
"name" : "value"
```

The field value can be a text string, a reference to another object, a function call, an expression, or an ordered list of any of the preceding types\. For more information about the types of data that can be used for field values, see [Simple Data Types](dp-expressions-functions.md#dp-pipeline-datatypes) \. For more information about functions that you can use to evaluate field values, see [Expression Evaluation](dp-pipeline-expressions.md#dp-datatype-functions)\. 

Fields are limited to 2048 characters\. Objects can be 20 KB in size, which means that you can't add many large fields to an object\.

Each pipeline object must contain the following fields: `id` and `type`, as shown in the following example\. Other fields may also be required based on the object type\. Select a value for `id` that's meaningful to you, and is unique within the pipeline definition\. The value for `type` specifies the type of the object\. Specify one of the supported pipeline definition object types, which are listed in the topic [Pipeline Object Reference](dp-pipeline-objects.md)\.

```
{
  "id": "MyCopyToS3",
  "type": "CopyActivity"
}
```

For more information about the required and optional fields for each object, see the documentation for the object\.

To include fields from one object in another object, use the `parent` field with a reference to the object\. For example, object "B" includes its fields, "B1" and "B2", plus the fields from object "A", "A1" and "A2"\.

```
{
  "id" : "A",
  "A1" : "value",
  "A2" : "value"
},
{
  "id" : "B",
  "parent" : {"ref" : "A"},
  "B1" : "value",
  "B2" : "value"
}
```

You can define common fields in an object with the ID "Default"\. These fields are automatically included in every object in the pipeline definition file that doesn't explicitly set its `parent` field to reference a different object\.

```
{
  "id" : "Default",
  "onFail" : {"ref" : "FailureNotification"},
  "maximumRetries" : "3",
  "workerGroup" : "myWorkerGroup"
}
```

## User\-Defined Fields<a name="dp-userdefined-fields"></a>

You can create user\-defined or custom fields on your pipeline components and refer to them with expressions\. The following example shows a custom field named `myCustomField` and `my_customFieldReference` added to an S3DataNode object:

```
{
  "id": "S3DataInput",
  "type": "S3DataNode",
  "schedule": {"ref": "TheSchedule"},
  "filePath": "s3://bucket_name",
  "myCustomField": "This is a custom value in a custom field.",
  "my_customFieldReference": {"ref":"AnotherPipelineComponent"}
  },
```

A user\-defined field must have a name prefixed with the word "my" in all lower\-case letters, followed by a capital letter or underscore character\. Additionally, a user\-defined field can be a string value such as the preceding `myCustomField` example, or a reference to another pipeline component such as the preceding `my_customFieldReference` example\.

**Note**  
On user\-defined fields, AWS Data Pipeline only checks for valid references to other pipeline components, not any custom field string values that you add\.