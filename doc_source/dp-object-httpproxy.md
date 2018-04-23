# HttpProxy<a name="dp-object-httpproxy"></a>

HttpProxy allows you to configure your own proxy and make Task Runner access the AWS Data Pipeline service through it\. You do not need to configure a running Task Runner with this information\.

## Example of an HttpProxy in TaskRunner<a name="example9"></a>

The following pipeline definition shows an `HttpProxy` object:

```
{
  "objects": [
    {
      "schedule": {
        "ref": "Once"
      },
      "pipelineLogUri": "s3://myDPLogUri/path",
      "name": "Default",
      "id": "Default"
    },
    {
      "name": "test_proxy",
      "hostname": "hostname",
      "port": "port",
      "username": "username",
      "*password": "password",
      "windowsDomain": "windowsDomain",
      "type": "HttpProxy",
      "id": "test_proxy",
    },
    {
      "name": "ShellCommand",
      "id": "ShellCommand",
      "runsOn": {
        "ref": "Resource"
      },
      "type": "ShellCommandActivity",
      "command": "echo 'hello world' "
    },
    {
      "period": "1 day",
      "startDateTime": "2013-03-09T00:00:00",
      "name": "Once",
      "id": "Once",
      "endDateTime": "2013-03-10T00:00:00",
      "type": "Schedule"
    },
    {
      "role": "dataPipelineRole",
      "httpProxy": {
        "ref": "test_proxy"
      },
      "actionOnResourceFailure": "retrynone",
      "maximumRetries": "0",
      "type": "Ec2Resource",
      "terminateAfter": "10 minutes",
      "resourceRole": "resourceRole",
      "name": "Resource",
      "actionOnTaskFailure": "terminate",
      "securityGroups": "securityGroups",
      "keyPair": "keyPair",
      "id": "Resource",
      "region": "us-east-1"
    }
  ],
  "parameters": []
}
```

## Syntax<a name="httpproxy-slots"></a>


****  

| Required Fields | Description | Slot Type | 
| --- | --- | --- | 
| hostname | Host of the proxy which clients will use to connect to AWS Services\. | String | 
| port | Port of the proxy host which the clients will use to connect to AWS Services\. | String | 


****  

| Optional Fields | Description | Slot Type | 
| --- | --- | --- | 
| parent | Parent of the current object from which slots will be inherited\. | Reference Object, e\.g\. "parent":\{"ref":"myBaseObjectId"\} | 
| \*password | Password for proxy\. | String | 
| s3NoProxy | Disable the HTTP proxy when connecting to Amazon S3 | Boolean | 
| username | User name for proxy\. | String | 
| windowsDomain | The Windows domain name for NTLM Proxy\. | String | 
| windowsWorkgroup | The Windows workgroup name for NTLM Proxy\. | String | 


****  

| Runtime Fields | Description | Slot Type | 
| --- | --- | --- | 
| @version | Pipeline version the object was created with\. | String | 


****  

| System Fields | Description | Slot Type | 
| --- | --- | --- | 
| @error | Error describing the ill\-formed object\. | String | 
| @pipelineId | Id of the pipeline to which this object belongs to\. | String | 
| @sphere | The sphere of an object denotes its place in the lifecycle: Component Objects give rise to Instance Objects which execute Attempt Objects\. | String | 