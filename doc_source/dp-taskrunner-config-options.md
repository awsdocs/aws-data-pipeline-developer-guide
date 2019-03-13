# Task Runner Configuration Options<a name="dp-taskrunner-config-options"></a>

These are the configuration options available from the command line when you launch Task Runner\. 


****  

| Command Line Parameter | Description | 
| --- | --- | 
| `--help` | Command line help\. Example: `Java -jar TaskRunner-1.0.jar --help` | 
| `--config` | The path and file name of your `credentials.json` file\. | 
| `--accessId` | Your AWS access key ID for Task Runner to use when making requests\. The `--accessID` and `--secretKey` options provide an alternative to using a credentials\.json file\. If a `credentials.json` file is also provided, the `--accessID` and `--secretKey` options take precedence\.  | 
| `--secretKey` | Your AWS secret key for Task Runner to use when making requests\. For more information, see `--accessID`\.  | 
| `--endpoint` | An endpoint is a URL that is the entry point for a web service\. The AWS Data Pipeline service endpoint in the region where you are making requests\. Optional\. In general, it is sufficient to specify a region, and you do not need to set the endpoint\. For a listing of AWS Data Pipeline regions and endpoints, see [AWS Data Pipeline Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#datapipeline_region) in the *AWS General Reference*\. | 
| `--workerGroup` | The name of the worker group for which Task Runner retrieves work\. Required\.When Task Runner polls the web service, it uses the credentials you supplied and the value of `workerGroup` to select which \(if any\) tasks to retrieve\. You can use any name that is meaningful to you; the only requirement is that the string must match between the Task Runner and its corresponding pipeline activities\. The worker group name is bound to a region\. Even if there are identical worker group names in other regions, Task Runner always get tasks from the region specified in `--region`\. | 
| `--taskrunnerId` | The ID of the task runner to use when reporting progress\. Optional\. | 
| `--output` | The Task Runner directory for log output files\. Optional\. Log files are stored in a local directory until they are pushed to Amazon S3\. This option overrides the default directory\.  | 
| `--region` | The region to use\. Optional, but it is recommended to always set the region\. If you do not specify the region, Task Runner retrieves tasks from the default service region, `us-east-1`\.Other supported regions are: `eu-west-1`, `ap-northeast-1`, `ap-southeast-2`, `us-west-2`\.  | 
| `--logUri` | The Amazon S3 destination path for Task Runner to back up log files to every hour\. When Task Runner terminates, active logs in the local directory are pushed to the Amazon S3 destination folder\.  | 
| \-\-proxyHost | The host of the proxy used by Task Runner clients to connect to AWS services\. | 
| \-\-proxyPort | Port of the proxy host used by Task Runner clients to connect to AWS services\. | 
| \-\-proxyUsername | The user name for proxy\. | 
| \-\-proxyPassword | The password for proxy\. | 
| \-\-proxyDomain | The Windows domain name for NTLM Proxy\. | 
| \-\-proxyWorkstation | The Windows workstation name for NTLM Proxy\. | 