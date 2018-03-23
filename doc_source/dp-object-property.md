# Property<a name="dp-object-property"></a>

A single key\-value property for use with an EmrConfiguration object\.

## Example<a name="property-example"></a>

The following pipeline definition shows an EmrConfiguration object and corresponding Property objects to launch an EmrCluster:

```
{
  "objects": [
    {
      "name": "ReleaseLabelCluster",
      "releaseLabel": "emr-4.1.0",
      "applications": ["spark", "hive", "pig"],
      "id": "ResourceId_I1mCc",
      "type": "EmrCluster",
      "configuration": {
        "ref": "coresite"
      }
    },
    {
      "name": "coresite",
      "id": "coresite",
      "type": "EmrConfiguration",
      "classification": "core-site",
      "property": [{
        "ref": "io-file-buffer-size"
      },
      {
        "ref": "fs-s3-block-size"
      }
      ]
    },
    {
      "name": "io-file-buffer-size",
      "id": "io-file-buffer-size",
      "type": "Property",
      "key": "io.file.buffer.size",
      "value": "4096"
    },
    {
      "name": "fs-s3-block-size",
      "id": "fs-s3-block-size",
      "type": "Property",
      "key": "fs.s3.block.size",
      "value": "67108864"
    }
  ]
}
```

## Syntax<a name="property-syntax"></a>

This object includes the following fields\.


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| key | key | String | 
| value | value | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| parent | Parent of the current object from which slots are inherited\. | Reference Object, for example, "parent":\{"ref":"myBaseObjectId"\} | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | Pipeline version that the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | ID of the pipeline to which this object belongs\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects, which execute Attempt Objects\. | String | 

## See Also<a name="property-seealso"></a>
+ [EmrCluster](dp-object-emrcluster.md)
+ [EmrConfiguration](dp-object-emrconfiguration.md)
+ [Amazon EMR Release Guide](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/)