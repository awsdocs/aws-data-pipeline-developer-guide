# Disable server\-side encryption on 3\.x releases<a name="emrcluster-example1-disable-encryption"></a>

**Example**  <a name="example3"></a>
An `EmrCluster` activity with a Hadoop version 2\.x created by AWS Data Pipeline enables server\-side encryption by default\. If you would like to disable server\-side encryption, you must specify a bootstrap action in the cluster object definition\.  
The following example creates an `EmrCluster` activity with server\-side encryption disabled:  

```
{  
   "id":"NoSSEEmrCluster",
   "type":"EmrCluster",
   "hadoopVersion":"2.x",
   "keyPair":"my-key-pair",
   "masterInstanceType":"m3.xlarge",
   "coreInstanceType":"m3.large",
   "coreInstanceCount":"10",
   "taskInstanceType":"m3.large",
   "taskInstanceCount":"10",
   "bootstrapAction":["s3://Region.elasticmapreduce/bootstrap-actions/configure-hadoop,-e, fs.s3.enableServerSideEncryption=false"]
}
```