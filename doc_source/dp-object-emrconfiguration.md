# EmrConfiguration<a name="dp-object-emrconfiguration"></a>

The EmrConfiguration object is the configuration used for EMR clusters with releases 4\.0\.0 or greater\. Configurations \(as a list\) is a parameter to the RunJobFlow API call\. The configuration API for Amazon EMR takes a classification and properties\. AWS Data Pipeline uses EmrConfiguration with corresponding Property objects to configure an [EmrCluster](dp-object-emrcluster.md) application such as Hadoop, Hive, Spark, or Pig on EMR clusters launched in a pipeline execution\. Because configuration can only be changed for new clusters, you cannot provide a EmrConfiguration object for existing resources\. For more information, see [http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/)\.

## Example<a name="emrconfiguration-example"></a>

The following configuration object sets the `io.file.buffer.size` and `fs.s3.block.size` properties in `core-site.xml`:

```
[
   {  
      "classification":"core-site",
      "properties":
      {
         "io.file.buffer.size": "4096",
         "fs.s3.block.size": "67108864"
      }
   }
]
```

The corresponding pipeline object definition uses a EmrConfiguration object and a list of Property objects in the `property` field:

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

The following example is a nested configuration used to set the Hadoop environment with the `hadoop-env` classification:

```
[
  {
    "classification": "hadoop-env",
    "properties": {},
    "configurations": [
      {
        "classification": "export",
        "properties": {
          "YARN_PROXYSERVER_HEAPSIZE": "2396"
        }
      }
    ]
  }
]
```

The corresponding pipeline definition object that uses this configuration is below:

```
{
  "objects": [
    {
      "name": "ReleaseLabelCluster",
      "releaseLabel": "emr-4.0.0",
      "applications": ["spark", "hive", "pig"],
      "id": "ResourceId_I1mCc",
      "type": "EmrCluster",
      "configuration": {
        "ref": "hadoop-env"
      }
    },
    {
      "name": "hadoop-env",
      "id": "hadoop-env",
      "type": "EmrConfiguration",
      "classification": "hadoop-env",
      "configuration": {
        "ref": "export"
      }
    },
    {
      "name": "export",
      "id": "export",
      "type": "EmrConfiguration",
      "classification": "export",
      "property": {
        "ref": "yarn-proxyserver-heapsize"
      }
    },
    {
      "name": "yarn-proxyserver-heapsize",
      "id": "yarn-proxyserver-heapsize",
      "type": "Property",
      "key": "YARN_PROXYSERVER_HEAPSIZE",
      "value": "2396"
    },
  ]
}
```

## Syntax<a name="emrconfiguration-syntax"></a>

This object includes the following fields\.


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| classification | Classification for the configuration\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| configuration | Sub\-configuration for this configuration\. | Reference Object, e\.g\. "configuration":\{"ref":"myEmrConfigurationId"\} | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| property | Configuration property\. | Reference Object, e\.g\. "property":\{"ref":"myPropertyId"\} | 


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

## See Also<a name="emrconfiguration-seealso"></a>
+ [EmrCluster](dp-object-emrcluster.md)
+ [Property](dp-object-property.md)
+ [Amazon EMR Release Guide](http://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/)