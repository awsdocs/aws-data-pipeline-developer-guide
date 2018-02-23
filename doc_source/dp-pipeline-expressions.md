# Expressions<a name="dp-pipeline-expressions"></a>

Expressions enable you to share a value across related objects\. Expressions are processed by the AWS Data Pipeline web service at runtime, ensuring that all expressions are substituted with the value of the expression\. 

Expressions are delimited by: "\#\{" and "\}"\. You can use an expression in any pipeline definition object where a string is legal\. If a slot is a reference or one of type ID, NAME, TYPE, SPHERE, its value is not evaluated and used verbatim\.

The following expression calls one of the AWS Data Pipeline functions\. For more information, see [Expression Evaluation](#dp-datatype-functions)\.

```
#{format(myDateTime,'YYYY-MM-dd hh:mm:ss')}
```

## Referencing Fields and Objects<a name="dp-pipeline-expressions-reference"></a>

Expressions can use fields of the current object where the expression exists, or fields of another object that is linked by a reference\.

A slot format consists of a creation time followed by the object creation time, such as `@S3BackupLocation_2018-01-31T11:05:33`\. 

 You can also reference the exact slot ID specified in the pipeline definition, such as the slot ID of the Amazon S3 backup location\. To reference the slot ID, use `#{parent.@id}`\.

In the following example, the `filePath` field references the `id` field in the same object to form a file name\. The value of `filePath` evaluates to "`s3://mybucket/ExampleDataNode.csv`"\. 

```
{
  "id" : "ExampleDataNode",
  "type" : "S3DataNode",
  "schedule" : {"ref" : "ExampleSchedule"},
  "filePath" : "s3://mybucket/#{parent.@id}.csv",
  "precondition" : {"ref" : "ExampleCondition"},
  "onFail" : {"ref" : "FailureNotify"}
}
```

To use a field that exists on another object linked by a reference, use the `node` keyword\. This keyword is only available with alarm and precondition objects\.

Continuing with the previous example, an expression in an `SnsAlarm` can refer to the date and time range in a `Schedule`, because the `S3DataNode` references both\.

 Specifically, `FailureNotify`'s `message` field can use the `@scheduledStartTime` and `@scheduledEndTime` runtime fields from `ExampleSchedule`, because `ExampleDataNode`'s `onFail` field references `FailureNotify` and its `schedule` field references `ExampleSchedule`\.

```
{  
    "id" : "FailureNotify",
    "type" : "SnsAlarm",
    "subject" : "Failed to run pipeline component",
    "message": "Error for interval #{node.@scheduledStartTime}..#{node.@scheduledEndTime}.",
    "topicArn":"arn:aws:sns:us-east-1:28619EXAMPLE:ExampleTopic"
},
```

**Note**  
You can create pipelines that have dependencies, such as tasks in your pipeline that depend on the work of other systems or tasks\. If your pipeline requires certain resources, add those dependencies to the pipeline using preconditions that you associate with data nodes and tasks\. This makes your pipelines easier to debug and more resilient\. Additionally, keep your dependencies within a single pipeline when possible, because cross\-pipeline troubleshooting is difficult\.

## Nested Expressions<a name="dp-datatype-nested"></a>

 AWS Data Pipeline allows you to nest values to create more complex expressions\. For example, to perform a time calculation \(subtract 30 minutes from the `scheduledStartTime`\) and format the result to use in a pipeline definition, you could use the following expression in an activity: 

```
#{format(minusMinutes(@scheduledStartTime,30),'YYYY-MM-dd hh:mm:ss')}
```

 and using the `node` prefix if the expression is part of an SnsAlarm or Precondition: 

```
#{format(minusMinutes(node.@scheduledStartTime,30),'YYYY-MM-dd hh:mm:ss')}
```

## Lists<a name="dp-datatype-list-function"></a>

Expressions can be evaluated on lists and functions on lists\. For example, assume that a list is defined like the following: `"myList":["one","two"]`\. If this list is used in the expression `#{'this is ' + myList}`, it will evaluate to `["this is one", "this is two"]`\. If you have two lists, Data Pipeline will ultimately flatten them in their evaluation\. For example, if `myList1` is defined as `[1,2]` and `myList2` is defined as `[3,4]` then the expression `[#{myList1}, #{myList2}]` will evaluate to `[1,2,3,4]`\.

## Node Expression<a name="dp-datatype-node"></a>

 AWS Data Pipeline uses the `#{node.*}` expression in either `SnsAlarm` or `PreCondition` for a back\-reference to a pipeline component's parent object\. Since `SnsAlarm` and `PreCondition` are referenced from an activity or resource with no reference back from them, `node` provides the way to refer to the referrer\. For example, the following pipeline definition demonstrates how a failure notification can use `node` to make a reference to its parent, in this case `ShellCommandActivity`, and include the parent's scheduled start and end times in the `SnsAlarm` message\. The scheduledStartTime reference on ShellCommandActivity does not require the `node` prefix because scheduledStartTime refers to itself\. 

**Note**  
The fields preceded by the AT \(@\) sign indicate those fields are runtime fields\.

```
{
  "id" : "ShellOut",
  "type" : "ShellCommandActivity",
  "input" : {"ref" : "HourlyData"},
  "command" : "/home/userName/xxx.sh #{@scheduledStartTime} #{@scheduledEndTime}",   
  "schedule" : {"ref" : "HourlyPeriod"},
  "stderr" : "/tmp/stderr:#{@scheduledStartTime}",
  "stdout" : "/tmp/stdout:#{@scheduledStartTime}",
  "onFail" : {"ref" : "FailureNotify"},
},
{  
  "id" : "FailureNotify",
  "type" : "SnsAlarm",
  "subject" : "Failed to run pipeline component",
  "message": "Error for interval #{node.@scheduledStartTime}..#{node.@scheduledEndTime}.",
  "topicArn":"arn:aws:sns:us-east-1:28619EXAMPLE:ExampleTopic"
},
```

AWS Data Pipeline supports transitive references for user\-defined fields, but not runtime fields\. A transitive reference is a reference between two pipeline components that depends on another pipeline component as the intermediary\. The following example shows a reference to a transitive user\-defined field and a reference to a non\-transitive runtime field, both of which are valid\. For more information, see [User\-Defined Fields](dp-writing-pipeline-definition.md#dp-userdefined-fields)\. 

```
{
  "name": "DefaultActivity1",
  "type": "CopyActivity",
  "schedule": {"ref": "Once"},
  "input": {"ref": "s3nodeOne"},  
  "onSuccess": {"ref": "action"},
  "workerGroup": "test",  
  "output": {"ref": "s3nodeTwo"}
},
{
  "name": "action",
  "type": "SnsAlarm",
  "message": "S3 bucket '#{node.output.directoryPath}' succeeded at #{node.@actualEndTime}.",
  "subject": "Testing",  
  "topicArn": "arn:aws:sns:us-east-1:28619EXAMPLE:ExampleTopic",
  "role": "DataPipelineDefaultRole"
}
```

## Expression Evaluation<a name="dp-datatype-functions"></a>

 AWS Data Pipeline provides a set of functions that you can use to calculate the value of a field\. The following example uses the `makeDate` function to set the `startDateTime` field of a `Schedule` object to `"2011-05-24T0:00:00"` GMT/UTC\. 

```
"startDateTime" : "makeDate(2011,5,24)"
```