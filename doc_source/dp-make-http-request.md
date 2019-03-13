# Making an HTTP Request to AWS Data Pipeline<a name="dp-make-http-request"></a>

For a complete description of the programmatic objects in AWS Data Pipeline, see the [AWS Data Pipeline API Reference](https://docs.aws.amazon.com/datapipeline/latest/APIReference/Welcome.html)\. 

 If you don't use one of the AWS SDKs, you can perform AWS Data Pipeline operations over HTTP using the POST request method\. The POST method requires you to specify the operation in the header of the request and provide the data for the operation in JSON format in the body of the request\. 

## HTTP Header Contents<a name="dp-http-header"></a>

 AWS Data Pipeline requires the following information in the header of an HTTP request: 
+  `host` The AWS Data Pipeline endpoint\. 

  For information about endpoints, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\. 
+  `x-amz-date` You must provide the time stamp in either the HTTP Date header or the AWS x\-amz\-date header\. \(Some HTTP client libraries don't let you set the Date header\.\) When an x\-amz\-date header is present, the system ignores any Date header during the request authentication\. 

   The date must be specified in one of the following three formats, as specified in the HTTP/1\.1 RFC: 
  +  Sun, 06 Nov 1994 08:49:37 GMT \(RFC 822, updated by RFC 1123\) 
  +  Sunday, 06\-Nov\-94 08:49:37 GMT \(RFC 850, obsoleted by RFC 1036\) 
  +  Sun Nov 6 08:49:37 1994 \(ANSI C asctime\(\) format\) 
+  `Authorization` The set of authorization parameters that AWS uses to ensure the validity and authenticity of the request\. For more information about constructing this header, go to [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. 
+  `x-amz-target` The destination service of the request and the operation for the data, in the format: `<<serviceName>>_<<API version>>.<<operationName>>` 

  For example, `DataPipeline_20121129.ActivatePipeline`
+  `content-type` Specifies JSON and the version\. For example, `Content-Type: application/x-amz-json-1.0` 

 The following is an example header for an HTTP request to activate a pipeline\. 

```
POST / HTTP/1.1
host: https://datapipeline.us-east-1.amazonaws.com
x-amz-date: Mon, 12 Nov 2012 17:49:52 GMT
x-amz-target: DataPipeline_20121129.ActivatePipeline
Authorization: AuthParams
Content-Type: application/x-amz-json-1.1
Content-Length: 39
Connection: Keep-Alive
```

## HTTP Body Content<a name="dp-http-body-content"></a>

 The body of an HTTP request contains the data for the operation specified in the header of the HTTP request\. The data must be formatted according to the JSON data schema for each AWS Data Pipeline API\. The AWS Data Pipeline JSON data schema defines the types of data and parameters \(such as comparison operators and enumeration constants\) available for each operation\. 

### Format the Body of an HTTP Request<a name="dp-format-http-body"></a>

 Use the JSON data format to convey data values and data structure, simultaneously\. Elements can be nested within other elements by using bracket notation\. The following example shows a request for putting a pipeline definition consisting of three objects and their corresponding slots\. 

```
{
 "pipelineId": "df-00627471SOVYZEXAMPLE",
 "pipelineObjects": 
  [
    {"id": "Default",
     "name": "Default",
     "slots": 
      [
        {"key": "workerGroup", 
         "stringValue": "MyWorkerGroup"}
      ]
    }, 
    {"id": "Schedule",
     "name": "Schedule",
     "slots": 
      [
       {"key": "startDateTime", 
         "stringValue": "2012-09-25T17:00:00"}, 
        {"key": "type", 
         "stringValue": "Schedule"}, 
        {"key": "period", 
         "stringValue": "1 hour"}, 
        {"key": "endDateTime", 
         "stringValue": "2012-09-25T18:00:00"}
      ]
    },
    {"id": "SayHello",
     "name": "SayHello",
     "slots": 
      [
        {"key": "type", 
         "stringValue": "ShellCommandActivity"},
        {"key": "command", 
         "stringValue": "echo hello"},
        {"key": "parent", 
         "refValue": "Default"},
        {"key": "schedule", 
         "refValue": "Schedule"}
 
      ]
    }
  ]
}
```

### Handle the HTTP Response<a name="dp-handle-http-responses"></a>

 Here are some important headers in the HTTP response, and how you should handle them in your application: 
+  **HTTP/1\.1**—This header is followed by a status code\. A code value of 200 indicates a successful operation\. Any other value indicates an error\. 
+  **x\-amzn\-RequestId**—This header contains a request ID that you can use if you need to troubleshoot a request with AWS Data Pipeline\. An example of a request ID is K2QH8DNOU907N97FNA2GDLL8OBVV4KQNSO5AEMVJF66Q9ASUAAJG\. 
+  **x\-amz\-crc32**—AWS Data Pipeline calculates a CRC32 checksum of the HTTP payload and returns this checksum in the x\-amz\-crc32 header\. We recommend that you compute your own CRC32 checksum on the client side and compare it with the x\-amz\-crc32 header; if the checksums do not match, it might indicate that the data was corrupted in transit\. If this happens, you should retry your request\. 

 AWS SDK users do not need to manually perform this verification, because the SDKs compute the checksum of each reply from Amazon DynamoDB and automatically retry if a mismatch is detected\. 

### Sample AWS Data Pipeline JSON Request and Response<a name="dp-json-sample-request-response"></a>

 The following examples show a request for creating a new pipeline\. Then it shows the AWS Data Pipeline response, including the pipeline identifier of the newly created pipeline\. 

#### HTTP POST Request<a name="dp-http-post-request"></a>

```
POST / HTTP/1.1
host: https://datapipeline.us-east-1.amazonaws.com
x-amz-date: Mon, 12 Nov 2012 17:49:52 GMT
x-amz-target: DataPipeline_20121129.CreatePipeline
Authorization: AuthParams
Content-Type: application/x-amz-json-1.1
Content-Length: 50
Connection: Keep-Alive

{"name": "MyPipeline",
 "uniqueId": "12345ABCDEFG"}
```

#### AWS Data Pipeline Response<a name="dp-http-post-response"></a>

```
HTTP/1.1 200 
x-amzn-RequestId: b16911ce-0774-11e2-af6f-6bc7a6be60d9
x-amz-crc32: 2215946753
Content-Type: application/x-amz-json-1.0
Content-Length: 2
Date: Mon, 16 Jan 2012 17:50:53 GMT

{"pipelineId": "df-00627471SOVYZEXAMPLE"}
```