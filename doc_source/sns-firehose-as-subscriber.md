# Fanout to Kinesis Data Firehose delivery streams<a name="sns-firehose-as-subscriber"></a>

You can subscribe [Amazon Kinesis Data Firehose delivery streams](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html) to SNS topics, which allows you to send notifications to additional storage and analytics endpoints\. Messages published to an SNS topic are sent to the subscribed Kinesis Data Firehose delivery stream, and delivered to the destination as configured in Kinesis Data Firehose\. A subscription owner can subscribe up to five Kinesis Data Firehose delivery streams to an SNS topic\.

Through Kinesis Data Firehose delivery streams, you can fan out Amazon SNS notifications to Amazon Simple Storage Service \(Amazon S3\), Amazon Redshift, Amazon OpenSearch Service \(OpenSearch Service\), and to third\-party service providers such as Datadog, New Relic, MongoDB, and Splunk\.

For example, you can use this functionality to permanently store messages sent to a topic in an Amazon S3 bucket for compliance, archival, or other purposes\. To do this, create a Kinesis Data Firehose delivery stream with an S3 bucket destination, and subscribe that delivery stream to the SNS topic\. As another example, to perform analysis on messages sent to an SNS topic, create a delivery stream with an OpenSearch Service index destination\. Then subscribe the Kinesis Data Firehose delivery stream to the SNS topic\.

Amazon SNS also supports message delivery status logging for notifications sent to Kinesis Data Firehose endpoints\. For more information, see [Amazon SNS message delivery status](sns-topic-attributes.md)\.

**Topics**
+ [Prerequisites](prereqs-kinesis-data-firehose.md)
+ [Subscribing a delivery stream to a topic](firehose-endpoints-subscribe.md)
+ [Delivery stream destinations](firehose-working-with-destinations.md)
+ [Example use case](firehose-example-use-case.md)