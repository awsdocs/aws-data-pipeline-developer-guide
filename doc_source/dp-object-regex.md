# RegEx Data Format<a name="dp-object-regex"></a>

A custom data format defined by a regular expression\.

## Example<a name="regex-example"></a>

The following is an example of this object type\. 

```
{
  "id" : "MyInputDataType",
  "type" : "RegEx",
  "inputRegEx" : "([^ ]*) ([^ ]*) ([^ ]*) (-|\\[[^\\]]*\\]) ([^ \"]*|\"[^\"]*\") (-|[0-9]*) (-|[0-9]*)(?: ([^ \"]*|\"[^\"]*\") ([^ \"]*|\"[^\"]*\"))?",
  "outputFormat" : "%1$s %2$s %3$s %4$s %5$s %6$s %7$s %8$s %9$s",
  "column" : [
    "host STRING",
    "identity STRING",
    "user STRING",
    "time STRING",
    "request STRING",
    "status STRING",
    "size STRING",
    "referer STRING",
    "agent STRING"
  ]
}
```

## Syntax<a name="regex-syntax"></a>


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| column | Column name with datatype specified by each field for the data described by this data node\. Ex: hostname STRING For multiple values, use column names and data types separated by a space\. | String | 
| inputRegEx | The regular expression to parse an S3 input file\. inputRegEx provides a way to retrieve columns from relatively unstructured data in a file\. | String | 
| outputFormat | The column fields retrieved by inputRegEx, but referenced as %1$s %2$s using Java formatter syntax\. | String | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 


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