# Example use case for message archiving and analytics<a name="firehose-example-use-case"></a>

This section provides a tutorial of a common use case for archiving and analyzing Amazon SNS messages\.

The setting of this use case is an airline ticketing platform that operates in a regulated environment\. The platform is subject to a compliance framework that requires the company to archive all ticket sales for at least five years\. To meet the compliance goal on data retention, the company subscribes an Amazon Kinesis Data Firehose delivery stream to an existing SNS topic\. The destination for the delivery stream is an Amazon Simple Storage Service \(Amazon S3\) bucket\. With this configuration, all events published to the SNS topic are archived in the Amazon S3 bucket\. The following diagram shows the architecture of this configuration:

![\[Example architecture of an airline ticketing platform.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-archiving-use-case.png)

To run analytics and gain insights on ticket sales, the company runs SQL queries using Amazon Athena\. For example, the company can query to learn about the most popular destinations and the most frequent flyers\.

To create the AWS resources for this use case, you can use the AWS Management Console or an AWS CloudFormation template\.

**Topics**
+ [Creating the initial resources](firehose-example-initial-resources.md)
+ [Creating the delivery stream](firehose-example-create-delivery-stream.md)
+ [Subscribing the delivery stream to the topic](firehose-example-subscribe-delivery-stream-to-topic.md)
+ [Testing and querying](firehose-example-test-and-query.md)
+ [Example \(AWS CloudFormation\)](firehose-example-cfn.md)