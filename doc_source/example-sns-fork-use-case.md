# Example AWS Event Fork Pipelines use case<a name="example-sns-fork-use-case"></a>

The following scenario describes an event\-driven, serverless e\-commerce application that uses AWS Event Fork Pipelines\. You can use this [example e\-commerce application](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:077246666028:applications~fork-example-ecommerce-checkout-api) in the AWS Serverless Application Repository and then deploy it in your AWS account using the AWS Lambda console, where you can test it and examine its source code in GitHub\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fork-example-use-case.png)

This e\-commerce application takes orders from buyers through a RESTful API hosted by API Gateway and backed by the AWS Lambda function `CheckoutApiBackendFunction`\. This function publishes all received orders to an Amazon SNS topic named `CheckoutEventsTopic` which, in turn, fans out the orders to four different pipelines\.

The first pipeline is the regular checkout\-processing pipeline designed and implemented by the owner of the e\-commerce application\. This pipeline has the Amazon SQS queue `CheckoutQueue` that buffers all received orders, an AWS Lambda function named `CheckoutFunction` that polls the queue to process these orders, and the DynamoDB table `CheckoutTable` that securely saves all placed orders\.

## Applying AWS Event Fork Pipelines<a name="applying-sns-fork-pipelines"></a>

The components of the e\-commerce application handle the core business logic\. However, the e\-commerce application owner also needs to address the following:
+ **Compliance**—secure, compressed backups encrypted at rest and sanitization of sensitive information
+ **Resiliency**—replay of most recent orders in case of the disruption of the fulfillment process
+ **Searchability**—running analytics and generating metrics on placed orders

Instead of implementing this event processing logic, the application owner can subscribe AWS Event Fork Pipelines to the `CheckoutEventsTopic` Amazon SNS topic
+ [The event storage and backup pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-storage-and-backup-pipeline) is configured to transform data to remove credit card details, buffer data for 60 seconds, compress it using GZIP, and encrypt it using the default Customer Master Key \(CMK\) for Amazon S3\. This CMK is managed by AWS and powered by the AWS Key Management Service \(AWS KMS\)\.

  For more information, see [Choose Amazon S3 For Your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-s3), [Amazon Kinesis Data Firehose Data Transformation](https://docs.aws.amazon.com/firehose/latest/dev/data-transformation.html), and [Configure Settings](https://docs.aws.amazon.com/firehose/latest/dev/create-configure.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.
+ [The event search and analytics pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-search-and-analytics-pipeline) is configured with an index retry duration of 30 seconds, a bucket for storing orders that fail to be indexed in the search domain, and a filter policy to restrict the set of indexed orders\.

  For more information, see [Choose Amazon ES for your Destination](https://docs.aws.amazon.com/firehose/latest/dev/create-destination.html#create-destination-elasticsearch) in the *Amazon Kinesis Data Firehose Developer Guide*\.
+ [The event replay pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-replay-pipeline) is configured with the Amazon SQS queue part of the regular order\-processing pipeline designed and implemented by the e\-commerce application owner\.

  For more information, see [Queue Name and URL](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-general-identifiers.html#queue-name-url) in the *Amazon Simple Queue Service Developer Guide*\.

The following JSON filter policy is set in the configuration for the Event Search and Analytics Pipeline\. It matches only incoming orders in which the total amount is $100 or higher\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

```
{
   "amount": [{ "numeric": [ ">=", 100 ] }]
}
```

Using the AWS Event Fork Pipelines pattern, the e\-commerce application owner can avoid the development overhead that often follows coding undifferentiating logic for event handling\. Instead, she can deploy AWS Event Fork Pipelines directly from the AWS Serverless Application Repository into her AWS account\.