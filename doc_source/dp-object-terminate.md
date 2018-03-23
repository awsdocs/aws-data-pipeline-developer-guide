# Terminate<a name="dp-object-terminate"></a>

An action to trigger the cancellation of a pending or unfinished activity, resource, or data node\. AWS Data Pipeline attempts to put the activity, resource, or data node into the CANCELLED state if it does not start by the `lateAfterTimeout` value\. 

You cannot terminate actions that include `onSuccess`, `OnFail`, or `onLateAction` resources\.

## Example<a name="terminate-example"></a>

The following is an example of this object type\. In this example, the `onLateAction` field of `MyActivity` contains a reference to the action `DefaultAction1`\. When you provide an action for `onLateAction`, you must also provide a `lateAfterTimeout` value to indicate the period of time since the scheduled start of the pipeline after which the activity is considered late\.

```
{
  "name" : "MyActivity",
  "id" : "DefaultActivity1",
  "schedule" : {
    "ref" : "MySchedule"
  },
  "runsOn" : {
    "ref" : "MyEmrCluster"
  },
  "lateAfterTimeout" : "1 Hours",
  "type" : "EmrActivity",
  "onLateAction" : {
    "ref" : "DefaultAction1"
  },
  "step" : [
    "s3://myBucket/myPath/myStep.jar,firstArg,secondArg",
    "s3://myBucket/myPath/myOtherStep.jar,anotherArg"
  ]
},
{
  "name" : "TerminateTasks",
  "id" : "DefaultAction1",
  "type" : "Terminate"
}
```

## Syntax<a name="terminate-syntax"></a>


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| parent | Parent of the current object from which slots are inherited\. | Reference Object, for example "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| node | The node for which this action is being performed\. | Reference Object, for example "node":\{"ref":"myRunnableObjectId"\} | 
| @version | Pipeline version that the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects, which execute Attempt Objects\. | String | 