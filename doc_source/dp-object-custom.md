# Custom Data Format<a name="dp-object-custom"></a>

A custom data format defined by a combination of a certain column separator, record separator, and escape character\.

## Example<a name="custom-example"></a>

The following is an example of this object type\. 

```
{
  "id" : "MyOutputDataType",
  "type" : "Custom",
  "columnSeparator" : ",",
  "recordSeparator" : "\n",
  "column" : [
    "Name STRING",
    "Score INT",
    "DateOfBirth TIMESTAMP"
  ]
}
```

## Syntax<a name="custom-syntax"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| columnSeparator | A character that indicates the end of a column in a data file\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| column | Column name with datatype specified by each field for the data described by this data node\. Ex: hostname STRING For multiple values, use column names and data types separated by a space\. | String | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| recordSeparator | A character that indicates the end of a row in a data file, for example "\\n"\. Only single characters are supported\. | String | 


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