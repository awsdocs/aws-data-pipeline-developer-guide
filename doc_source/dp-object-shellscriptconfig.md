# ShellScriptConfig<a name="dp-object-shellscriptconfig"></a>

Use with an Activity to run a shell script for preActivityTaskConfig and postActivityTaskConfig\. This object is available for HadoopActivity, HiveActivity, HiveCopyActivity, and PigActivity\. You specify an S3 URI and a list of arguments for the script\.

## Example<a name="shellscriptconfig-example"></a>

A ShellScriptConfig with arguments:

```
{
   "id" : "ShellScriptConfig_1”,
   "name" : “prescript”,
   "type" : "ShellScriptConfig",
   "scriptUri": “s3://my-bucket/shell-cleanup.sh”,
   "scriptArgument" : ["arg1","arg2"]
 }
```

## Syntax<a name="shellscriptconfig-syntax"></a>

This object includes the following fields\.


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| scriptArgument | A list of argument\(s\) to use with the shell script\. | String | 
| scriptUri | The script URI in Amazon S3 that should be downloaded and run\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | Pipeline version the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object | String | 
| @pipelineId | Id of the pipeline to which this object belongs to | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects | String | 