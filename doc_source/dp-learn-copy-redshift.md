# Before You Begin: Configure COPY Options and Load Data<a name="dp-learn-copy-redshift"></a>

Before copying data to Amazon Redshift within AWS Data Pipeline, ensure that you: 
+ Load data from Amazon S3\.
+ Set up the `COPY` activity in Amazon Redshift\. 

Once you have these options working and successfully complete a data load, transfer these options to AWS Data Pipeline, for performing the copying within it\.

 For `COPY` options, see [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the Amazon Redshift *Database Developer Guide*\. 

For steps to load data from Amazon S3, see [Loading data from Amazon S3](https://docs.aws.amazon.com/redshift/latest/dg/t_Loading-data-from-S3.html) in the Amazon Redshift *Database Developer Guide*\. 

For example, the following SQL command in Amazon Redshift creates a new table named `LISTING` and copies sample data from a publicly available bucket in Amazon S3\. 

Replace the `<iam-role-arn>` and region with your own\. 

For details about this example, see [Load Sample Data from Amazon S3](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-create-sample-db.html) in the Amazon Redshift *Getting Started Guide*\.

```
create table listing(
	listid integer not null distkey,
	sellerid integer not null,
	eventid integer not null,
	dateid smallint not null  sortkey,
	numtickets smallint not null,
	priceperticket decimal(8,2),
	totalprice decimal(8,2),
	listtime timestamp);

copy listing from 's3://awssampledbuswest2/tickit/listings_pipe.txt' 
credentials 'aws_iam_role=<iam-role-arn>' 
delimiter '|' region 'us-west-2';
```