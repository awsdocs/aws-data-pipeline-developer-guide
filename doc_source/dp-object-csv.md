# CSV Data Format<a name="dp-object-csv"></a>

A comma\-delimited data format where the column separator is a comma and the record separator is a newline character\.

## Example<a name="csv-example"></a>

The following is an example of this object type\. 

```
{
  "id" : "MyOutputDataType",
  "type" : "CSV",
  "column" : [
    "Name STRING",
    "Score INT",
    "DateOfBirth TIMESTAMP"
  ]
}
```

## Syntax<a name="csv-syntax"></a>


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| column | Column name with datatype specified by each field for the data described by this data node\. Ex: hostname STRING For multiple values, use column names and data types separated by a space\. | String | 
| escapeChar | A character, for example "\\", that instructs the parser to ignore the next character\. | String | 
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