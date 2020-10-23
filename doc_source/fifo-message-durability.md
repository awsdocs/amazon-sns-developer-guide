# Message durability for FIFO topics<a name="fifo-message-durability"></a>

Amazon SNS FIFO topics and Amazon SQS FIFO queues are durable\. Both resource types store messages redundantly across multiple Availability Zones, and provide dead\-letter queues to handle exceptional cases\.

In Amazon SNS, message delivery fails when the Amazon SNS topic can't access a subscribed Amazon SQS queue due to a client\-side or server\-side error:
+ Client\-side errors occur when the SNS FIFO topic has stale subscription metadata\. Two common casues of client\-side errors are when the SQS FIFO queue owner does one of the following:
  + Deletes the queue\.
  + Changes the queue policy in a way that prevents the Amazon SNS service principal from delivering messages to it\.

  Amazon SNS doesn't retry deliverying messages that failed due to client\-side errors\.
+ Server\-side errors can occur in these situations:
  + The Amazon SQS service is unavailable\.
  + Amazon SQS fails to process a valid request from the Amazon SNS service\.

  When server\-side errors occur, SNS FIFO topics retry the failed deliveries up to 100,015 times over 23 days\. For more information, see [Amazon SNS message delivery retries](sns-message-delivery-retries.md)\.

For any type of error, Amazon SNS can sideline messages to Amazon SQS dead\-letter queues so data isn't lost\.

In Amazon SQS, message processing fails when the consumer application fails to receive the message, process it, and delete it from the queue\. When the maximum number of receive requests fail, Amazon SQS can sideline messages to dead\-letter queues so data isn't lost\.

In the [auto parts price management example use case](fifo-example-use-case.md), the company can assign an SQS FIFO dead\-letter queue \(DLQ\) to each SNS FIFO topic subscription, as well as to each subscribed SQS FIFO queue\. This protects the company from any price update loss\.

![\[Configure Amazon SQS dead-letter queues to make sure that messages are not lost.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-dlq.png)

The dead\-letter queue associated with an SNS FIFO subscription, or with an SQS FIFO queue, must be an SQS FIFO queue\. The dead\-letter queue must be in the same AWS Region and AWS account as the SNS FIFO subscription or SQS FIFO queue that it protects\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md) and the [ Designing durable serverless apps with DLQs for Amazon SNS, Amazon SQS, AWS Lambda](https://aws.amazon.com/blogs/compute/designing-durable-serverless-apps-with-dlqs-for-amazon-sns-amazon-sqs-aws-lambda/ ) post on the *AWS Compute Blog*\.