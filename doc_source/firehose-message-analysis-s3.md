# Analyzing messages for Amazon S3 destinations<a name="firehose-message-analysis-s3"></a>

This page describes how to analyze Amazon SNS messages sent through Amazon Kinesis Data Firehose delivery streams to Amazon Simple Storage Service \(Amazon S3\) destinations\.

**To analyze SNS messages sent through Kinesis Data Firehose delivery streams to Amazon S3 destinations**

1. Configure your Amazon S3 resources\. For instructions, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service User Guide* and [Working with Amazon S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) in the *Amazon Simple Storage Service User Guide*\.

1. Configure your delivery stream\. For instructions, see [Choose Amazon S3 for Your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-s3) in the *Amazon Kinesis Data Firehose Developer Guide*\.

1. Use [Amazon Athena](https://console.aws.amazon.com/athena) to query the Amazon S3 objects using standard SQL\. For more information, see [Getting Started](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html) in the *Amazon Athena User Guide*\.

## Example query<a name="example-s3-query"></a>

For this example query, assume the following:
+ Messages are stored in the `notifications` table in the `default` schema\.
+ The `notifications` table includes a `timestamp` column with a type of `string`\.

The following query returns all SNS messages received in the specified date range:

```
SELECT * 
FROM default.notifications
WHERE from_iso8601_timestamp(timestamp) BETWEEN TIMESTAMP '2020-12-01 00:00:00' AND TIMESTAMP '2020-12-02 00:00:00';
```