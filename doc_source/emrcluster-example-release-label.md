# Launch an Amazon EMR cluster with release label emr\-4\.x or greater<a name="emrcluster-example-release-label"></a>

**Example**  
The following example launches an Amazon EMR cluster using the newer `releaseLabel` field:  

```
{
  "id" : "MyEmrCluster",
  "type" : "EmrCluster",
  "keyPair" : "my-key-pair",
  "masterInstanceType" : "m3.xlarge",
  "coreInstanceType" : "m3.xlarge",
  "coreInstanceCount" : "10",
  "taskInstanceType" : "m3.xlarge",
  "taskInstanceCount": "10",
  "releaseLabel": "emr-4.1.0",
  "applications": ["spark", "hive", "pig"],
  "configuration": {"ref":"myConfiguration"}  
}
```