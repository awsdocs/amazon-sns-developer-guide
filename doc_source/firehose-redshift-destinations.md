# Amazon Redshift destinations<a name="firehose-redshift-destinations"></a>

This section describes how to fan out Amazon SNS notifications to an Amazon Kinesis Data Firehose delivery stream that publishes data to Amazon Redshift\. With this configuration, you can connect to the Amazon Redshift database and use a SQL query tool to query the database for Amazon SNS messages that meet certain criteria\.

![\[A publisher sends a message to an Amazon SNS topic, and the message is sent through Kinesis Data Firehose to a subscribed Amazon Redshift cluster.\]](http://docs.aws.amazon.com/sns/latest/dg/images/firehose-architecture-rs.png)

**Topics**
+ [Archive table structure for Amazon Redshift destinations](firehose-archive-table-structure-redshift.md)
+ [Analyzing messages for Amazon Redshift destinations](firehose-message-analysis-redshift.md)