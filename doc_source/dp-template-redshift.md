# Amazon RDS to Amazon Redshift Templates<a name="dp-template-redshift"></a>

The following two templates copy tables from Amazon RDS MySQL to Amazon Redshift using a translation script, which creates an Amazon Redshift table using the source table schema with the following caveats: 
+ If a distribution key is not specified, the first primary key from the Amazon RDS table is set as the distribution key\.
+ You cannot skip a column that is present in an Amazon RDS MySQL table when you are doing a copy to Amazon Redshift\.
+ \(Optional\) You can provide an Amazon RDS MySQL to Amazon Redshift column data type mapping as one of the parameters in the template\. If this is specified, the script uses this to create the Amazon Redshift table\.

If the `Overwrite_Existing` Amazon Redshift insert mode is being used:
+ If a distribution key is not provided, a primary key on the Amazon RDS MySQL table is used\.
+ If there are composite primary keys on the table, the first one is used as the distribution key if the distribution key is not provided\. Only the first composite key is set as the primary key in the Amazon Redshift table\.
+ If a distribution key is not provided and there is no primary key on the Amazon RDS MySQL table, the copy operation fails\.

For more information about Amazon Redshift, see the following topics:
+ [Amazon Redshift Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-security-groups.html)
+ Amazon Redshift [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html)
+ [Distribution styles](http://docs.aws.amazon.com/redshift/latest/dg/c_choosing_dist_sort.html) and DISTKEY [examples](http://docs.aws.amazon.com/redshift/latest/dg/c_Distribution_examples.html)
+ [Sort Keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)

The following table describes how the script translates the data types:


**Data Type Translations Between MySQL and Amazon Redshift**  

| **MySQL Data Type** | **Amazon Redshift Data Type ** | **Notes** | 
| --- | --- | --- | 
|  TINYINT, TINYINT \(size\)  | SMALLINT |  MySQL: \-128 to 127\. The maximum number of digits may be specified in parentheses\.  Amazon Redshift: INT2\. Signed two\-byte integer  | 
|  TINYINT UNSIGNED, TINYINT \(size\) UNSIGNED  | SMALLINT |  MySQL: 0 to 255 UNSIGNED\. The maximum number of digits may be specified in parentheses\. Amazon Redshift: INT2\. Signed two\-byte integer  | 
|  SMALLINT, SMALLINT\(size\)  | SMALLINT |  MySQL: \-32768 to 32767 normal\. The maximum number of digits may be specified in parentheses\. Amazon Redshift: INT2\. Signed two\-byte integer  | 
|  SMALLINT UNSIGNED, SMALLINT\(size\) UNSIGNED,  | INTEGER |  MySQL: 0 to 65535 UNSIGNED\*\. The maximum number of digits may be specified in parentheses Amazon Redshift: INT4\. Signed four\-byte integer  | 
|  MEDIUMINT, MEDIUMINT\(size\)  | INTEGER |  MySQL: 388608 to 8388607\. The maximum number of digits may be specified in parentheses Amazon Redshift: INT4\. Signed four\-byte integer  | 
|  MEDIUMINT UNSIGNED, MEDIUMINT\(size\) UNSIGNED  |  INTEGER  |  MySQL: 0 to 16777215\. The maximum number of digits may be specified in parentheses Amazon Redshift: INT4\. Signed four\-byte integer  | 
|  INT, INT\(size\)  | INTEGER |  MySQL: 147483648 to 2147483647 Amazon Redshift: INT4\. Signed four\-byte integer  | 
|  INT UNSIGNED, INT\(size\) UNSIGNED  | BIGINT |  MySQL: 0 to 4294967295 Amazon Redshift: INT8\. Signed eight\-byte integer  | 
|  BIGINT BIGINT\(size\)  | BIGINT |  Amazon Redshift: INT8\. Signed eight\-byte integer  | 
|  BIGINT UNSIGNED BIGINT\(size\) UNSIGNED  | VARCHAR\(20\*4\) |  MySQL: 0 to 18446744073709551615  Amazon Redshift: No native equivalent, so using char array\.  | 
|  FLOAT FLOAT\(size,d\) FLOAT\(size,d\) UNSIGNED  | REAL |  The maximum number of digits may be specified in the size parameter\. The maximum number of digits to the right of the decimal point is specified in the d parameter\. Amazon Redshift: FLOAT4  | 
|  DOUBLE\(size,d\)  | DOUBLE PRECISION |  The maximum number of digits may be specified in the size parameter\. The maximum number of digits to the right of the decimal point is specified in the d parameter\. Amazon Redshift: FLOAT8  | 
| DECIMAL\(size,d\) |  DECIMAL\(size,d\)  |  A DOUBLE stored as a string, allowing for a fixed decimal point\. The maximum number of digits may be specified in the size parameter\. The maximum number of digits to the right of the decimal point is specified in the d parameter\. Amazon Redshift: No native equivalent\.  | 
|  CHAR\(size\)  | VARCHAR\(size\*4\) |  Holds a fixed\-length string, which can contain letters, numbers, and special characters\. The fixed size is specified as the parameter in parentheses\. Can store up to 255 characters\. Right padded with spaces\. Amazon Redshift: CHAR data type does not support multibyte character so VARCHAR is used\. The maximum number of bytes per character is 4 according to [RFC3629](http://tools.ietf.org/html/rfc3629), which limits the character table to U\+10FFFF\.  | 
| VARCHAR\(size\) | VARCHAR\(size\*4\) |  Can store up to 255 characters\. VARCHAR does not support the following invalid UTF\-8 code points: 0xD800\- 0xDFFF, \(Byte sequences: ED A0 80\- ED BF BF\), 0xFDD0\- 0xFDEF, 0xFFFE, and 0xFFFF, \(Byte sequences: EF B7 90\- EF B7 AF, EF BF BE, and EF BF BF\)  | 
| TINYTEXT | VARCHAR\(255\*4\) | Holds a string with a maximum length of 255 characters | 
| TEXT | VARCHAR\(max\) |  Holds a string with a maximum length of 65,535 characters\.  | 
| MEDIUMTEXT | VARCHAR\(max\) |  0 to 16,777,215 Chars  | 
| LONGTEXT | VARCHAR\(max\) | 0 to 4,294,967,295 Chars | 
|  BOOLEAN BOOL TINYINT\(1\)  | BOOLEAN |  MySQL: These types are synonyms for [TINYINT\(1\)](http://dev.mysql.com/doc/refman/5.0/en/integer-types.html)\. A value of zero is considered false\. Nonzero values are considered true\.  | 
| BINARY\[\(M\)\] | varchar\(255\) |  M is 0 to 255 bytes, FIXED  | 
| VARBINARY\(M\) | VARCHAR\(max\) |  0 to 65,535 bytes  | 
| TINYBLOB | VARCHAR\(255\) | 0 to 255 bytes | 
| BLOB | VARCHAR\(max\) |  0 to 65,535 bytes  | 
| MEDIUMBLOB | VARCHAR\(max\) |  0 to 16,777,215 bytes  | 
| LONGBLOB | VARCHAR\(max\) |  0 to 4,294,967,295 bytes  | 
| ENUM | VARCHAR\(255\*2\) | The limit is not on the length of the literal enum string, but rather on the table definition for number of enum values\. | 
| SET | VARCHAR\(255\*2\) | Like enum\. | 
| DATE | DATE |  \(YYYY\-MM\-DD\) "1000\-01\-01" to "9999\-12\-31"  | 
| TIME | VARCHAR\(10\*4\) |  \(hh:mm:ss\) "\-838:59:59" to "838:59:59"  | 
| DATETIME | TIMESTAMP |  \(YYYY\-MM\-DD hh:mm:ss\) 1000\-01\-01 00:00:00" to "9999\-12\-31 23:59:59"  | 
| TIMESTAMP | TIMESTAMP |  \(YYYYMMDDhhmmss\) 19700101000000 to 2037\+  | 
| YEAR | VARCHAR\(4\*4\) |  \(YYYY\) 1900 to 2155  | 
|  column SERIAL  |  ID generation / This attribute is not needed for an OLAP data warehouse since this column is copied\. SERIAL keyword is not added while translating\.  |  SERIAL is in fact an entity named SEQUENCE\. It exists independently on the rest of your table\. column GENERATED BY DEFAULT equivalent to: CREATE SEQUENCE name; CREATE TABLE table \( column INTEGER NOT NULL DEFAULT nextval\(name\) \);  | 
| column BIGINT UNSIGNED NOT NULL AUTO\_INCREMENT UNIQUE |  ID generation / This attribute is not needed for OLAP data warehouse since this column is copied\. So SERIAL keyword is not added while translating\.  |  SERIAL is in fact an entity named SEQUENCE\. It exists independently on the rest of your table\. column GENERATED BY DEFAULT equivalent to: CREATE SEQUENCE name; CREATE TABLE table \( column INTEGER NOT NULL DEFAULT nextval\(name\) \);  | 
| ZEROFILL | ZEROFILL keyword is not added while translating\. |  INT UNSIGNED ZEROFILL NOT NULL ZEROFILL pads the displayed value of the field with zeros up to the display width specified in the column definition\. Values longer than the display width are not truncated\. Note that usage of ZEROFILL also implies UNSIGNED\.  | 