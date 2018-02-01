# Special Characters<a name="dp-pipeline-characters"></a>

AWS Data Pipeline uses certain characters that have a special meaning in pipeline definitions, as shown in the following table\. 


****  

| Special Character | Description | Examples | 
| --- | --- | --- | 
| @ | Runtime field\. This character is a field name prefix for a field that is only available when a pipeline runs\. | @actualStartTime @failureReason @resourceStatus | 
| \# | Expression\. Expressions are delimited by: "\#\{" and "\}" and the contents of the braces are evaluated by AWS Data Pipeline\. For more information, see [Expressions](dp-pipeline-expressions.md)\. | \#\{format\(myDateTime,'YYYY\-MM\-dd hh:mm:ss'\)\} s3://mybucket/\#\{id\}\.csv | 
| \* | Encrypted field\. This character is a field name prefix to indicate that AWS Data Pipeline should encrypt the contents of this field in transit between the console or CLI and the AWS Data Pipeline service\. | \*password | 