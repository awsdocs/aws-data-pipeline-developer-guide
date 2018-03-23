# Pipeline Expressions and Functions<a name="dp-expressions-functions"></a>

This section explains the syntax for using expressions and functions in pipelines, including the associated data types\.

## Simple Data Types<a name="dp-pipeline-datatypes"></a>

The following types of data can be set as field values\.

**Topics**
+ [DateTime](#dp-datatype-datetime)
+ [Numeric](#dp-datatype-numeric)
+ [Object References](#dp-datatype-object-reference)
+ [Period](#dp-datatype-period)
+ [String](#dp-datatype-section)

### DateTime<a name="dp-datatype-datetime"></a>

 AWS Data Pipeline supports the date and time expressed in "YYYY\-MM\-DDTHH:MM:SS" format in UTC/GMT only\. The following example sets the `startDateTime` field of a `Schedule` object to `1/15/2012, 11:59 p.m.`, in the UTC/GMT timezone\. 

```
"startDateTime" : "2012-01-15T23:59:00"
```

### Numeric<a name="dp-datatype-numeric"></a>

 AWS Data Pipeline supports both integers and floating\-point values\. 

### Object References<a name="dp-datatype-object-reference"></a>

An object in the pipeline definition\. This can either be the current object, the name of an object defined elsewhere in the pipeline, or an object that lists the current object in a field, referenced by the `node` keyword\. For more information about `node`, see [Referencing Fields and Objects](dp-pipeline-expressions.md#dp-pipeline-expressions-reference)\. For more information about the pipeline object types, see [Pipeline Object Reference](dp-pipeline-objects.md)\. 

### Period<a name="dp-datatype-period"></a>

 Indicates how often a scheduled event should run\. It's expressed in the format "*N* \[`years`\|`months`\|`weeks`\|`days`\|`hours`\|`minutes`\]", where *N* is a positive integer value\. 

The minimum period is 15 minutes and the maximum period is 3 years\.

The following example sets the `period` field of the `Schedule` object to 3 hours\. This creates a schedule that runs every three hours\.

```
"period" : "3 hours"
```

### String<a name="dp-datatype-section"></a>

 Standard string values\. Strings must be surrounded by double quotes \("\)\. You can use the backslash character \(\\\) to escape characters in a string\. Multiline strings are not supported\. 

The following examples show examples of valid string values for the `id` field\.

```
"id" : "My Data Object"

"id" : "My \"Data\" Object"
```

Strings can also contain expressions that evaluate to string values\. These are inserted into the string, and are delimited with: "\#\{" and "\}"\. The following example uses an expression to insert the name of the current object into a path\.

```
"filePath" : "s3://myBucket/#{name}.csv"
```

For more information about using expressions, see [Referencing Fields and Objects](dp-pipeline-expressions.md#dp-pipeline-expressions-reference) and [Expression Evaluation](dp-pipeline-expressions.md#dp-datatype-functions)\.