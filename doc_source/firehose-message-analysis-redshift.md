# Analyzing messages for Amazon Redshift destinations<a name="firehose-message-analysis-redshift"></a>

This page describes how to analyze Amazon SNS messages sent through Amazon Kinesis Data Firehose delivery streams to Amazon Redshift destinations\.

**To analyze SNS messages sent through Kinesis Data Firehose delivery streams to Amazon Redshift destinations**

1. Configure your Amazon Redshift resources\. For instructions, see [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html) in the *Amazon Redshift Getting Started Guide*\.

1. Configure your delivery stream\. For instructions, see [Choose Amazon Redshift for Your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-redshift) in the *Amazon Kinesis Data Firehose Developer Guide*\.

1. Run a query\. For more information, see [Querying a database using the query editor](https://docs.aws.amazon.com/redshift/latest/mgmt/query-editor.html) in the *Amazon Redshift Cluster Management Guide*\.

## Example query<a name="example-rs-query"></a>

For this example query, assume the following:
+ Messages are stored in the `notifications` table in the default `public` schema\.
+ The `Timestamp` property from the SNS message is stored in the table's `timestamp` column with a column data type of `timestamptz`\.
**Note**  
To transform the JSON metadata for the Amazon Redshift endpoint, you can use the SQL `COPY` command\. For more information, see [Copy from JSON examples](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#r_COPY_command_examples-copy-from-json) and [Load from JSON data using the 'auto ignorecase' option](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#copy-from-json-examples-using-auto-ignorecase) in the *Amazon Redshift Database Developer Guide*\.

The following query returns all SNS messages received in the specified date range:

```
SELECT *
FROM public.notifications
WHERE timestamp > '2020-12-01T09:00:00.000Z' AND timestamp < '2020-12-02T09:00:00.000Z';
```