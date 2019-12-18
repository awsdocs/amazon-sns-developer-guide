# Amazon SNS Dead\-Letter Queues<a name="sns-dead-letter-queues"></a>

A dead\-letter queue is an Amazon SQS queue that an Amazon SNS subscription can target for messages that can't be delivered to subscribers successfully\. Messages that can't be delivered due to client errors or server errors are held in the dead\-letter queue for further analysis or reprocessing\. For more information, see [Tutorial: Configuring an Amazon SNS Dead\-Letter Queue for a Subscription](sns-configure-dead-letter-queue.md) and [Message Delivery Retries](sns-message-delivery-retries.md)\.

**Note**  
The Amazon SNS subscription and Amazon SQS queue must be under the same AWS account and Region\.
Currently, you can't use an Amazon SQS FIFO queue as a dead\-letter queue for an Amazon SNS subscription\.
To use an encrypted Amazon SQS queue as a dead\-letter queue, you must use a custom CMK with a key policy that grants the Amazon SNS service principal access to AWS KMS API actions\. For more information, see [Encryption at Rest](sns-server-side-encryption.md) in this guide and [Protecting Amazon SQS Data Using Server\-Side Encryption \(SSE\) and AWS KMS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-server-side-encryption.html) in the *Amazon Simple Queue Service Developer Guide*\.

**Topics**
+ [Why Do Message Deliveries Fail?](#why-do-message-deliveries-fail)
+ [How Do Dead\-Letter Queues Work?](#how-do-dead-letter-queues-work)
+ [How Are Messages Moved into a Dead\-Letter Queue?](#how-messages-moved-into-dead-letter-queue)
+ [How Can I Move Messages out of a Dead\-Letter Queue?](#how-to-move-messages-out-of-dead-letter-queue)
+ [How Can I Monitor and Log Dead\-Letter Queues?](#how-to-monitor-log-dead-letter-queues)

## Why Do Message Deliveries Fail?<a name="why-do-message-deliveries-fail"></a>

In general, message delivery fails when Amazon SNS can't access a subscribed endpoint due to a *client\-side* or *server\-side error*\. When Amazon SNS receives a client\-side error, or continues to receive a server\-side error for a message beyond the number of retries specified by the corresponding retry policy, Amazon SNS discards the message—unless a dead\-letter queue is attached to the subscription\. Failed deliveries don't change the status of your subscriptions\. For more information, see [Message Delivery Retries](sns-message-delivery-retries.md)\.

### Client\-Side Errors<a name="client-side-errors"></a>

Client\-side errors can happen when Amazon SNS has stale subscription metadata\. These errors commonly occur when an owner deletes the endpoint \(for example, a Lambda function subscribed to an Amazon SNS topic\) or when an owner changes the policy attached to the subscribed endpoint in a way that prevents Amazon SNS from delivering messages to the endpoint\. Amazon SNS doesn't retry the message delivery that fails as a result of a client\-side error\.

### Server\-Side Errors<a name="server-side-errors"></a>

Server\-side errors can happen when the system responsible for the subscribed endpoint becomes unavailable or returns an exception that indicates that it can't process a valid request from Amazon SNS\. When server\-side errors occur, Amazon SNS retries the failed deliveries using either a linear or exponential backoff function\. For server\-side errors caused by AWS managed endpoints backed by Amazon SQS or AWS Lambda, Amazon SNS retries delivery up to 100,015 times, over 23 days\.

Customer managed endpoints \(such as HTTP, SMTP, SMS, or mobile push\) can also cause server\-side errors\. Amazon SNS retries delivery to these types of endpoints as well\. While HTTP endpoints support customer\-defined retry policies, Amazon SNS sets an internal delivery retry policy to 50 times over 6 hours, for SMTP, SMS, and mobile push endpoints\.

## How Do Dead\-Letter Queues Work?<a name="how-do-dead-letter-queues-work"></a>

A dead\-letter queue is attached to an Amazon SNS subscription \(rather than a topic\) because message deliveries happen at the subscription level\. This lets you identify the original target endpoint for each message more easily\.

A dead\-letter queue associated with an Amazon SNS subscription is an ordinary Amazon SQS queue\. For more information about the message retention period, see [Quotas Related to Messages](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-quotas.html#quotas-messages) in the *Amazon Simple Queue Service Developer Guide*\. You can change the message retention period using the Amazon SQS `[SetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SetQueueAttributes.html)` API action\. To make your applications more resilient, we recommend setting the maximum retention period for dead\-letter queues to 14 days\.

## How Are Messages Moved into a Dead\-Letter Queue?<a name="how-messages-moved-into-dead-letter-queue"></a>

Your messages are moved into a dead\-letter queue using a *redrive policy*\. A redrive policy is a JSON object that refers to the ARN of the dead\-letter queue\. The `deadLetterTargetArn` attribute specifies the ARN\. The ARN must point to an Amazon SQS queue in the same AWS account and Region as your Amazon SNS subscription\. For more information, see [Tutorial: Configuring an Amazon SNS Dead\-Letter Queue for a Subscription](sns-configure-dead-letter-queue.md)\. 

**Note**  
Currently, you can't use an Amazon SQS FIFO queue as a dead\-letter queue for an Amazon SNS subscription\.

The following JSON object is a sample redrive policy, attached to an SNS subscription\.

```
{
  "deadLetterTargetArn": "arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue"
}
```

## How Can I Move Messages out of a Dead\-Letter Queue?<a name="how-to-move-messages-out-of-dead-letter-queue"></a>

You can move messages out of a dead\-letter queue in two ways:
+ **Avoid writing Amazon SQS consumer logic** – Set your dead\-letter queue as an event source to the Lambda function to drain your dead\-letter queue\.
+ **Write Amazon SQS consumer logic** – Use the Amazon SQS API, AWS SDK, or AWS CLI to write custom consumer logic for polling, processing, and deleting the messages in the dead\-letter queue\.

## How Can I Monitor and Log Dead\-Letter Queues?<a name="how-to-monitor-log-dead-letter-queues"></a>

You can use Amazon CloudWatch metrics to monitor dead\-letter queues associated with your Amazon SNS subscriptions\. All Amazon SQS queues emit CloudWatch metrics at five\-minute intervals\. For more information, see [Available CloudWatch Metrics for Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-available-cloudwatch-metrics.html) in the *Amazon Simple Queue Service Developer Guide*\. All Amazon SNS subscriptions with dead\-letter queues also emit CloudWatch metrics\. For more information, see [Monitoring Amazon SNS Topics Using CloudWatch](sns-monitoring-using-cloudwatch.md)\.

To be notified of activity in your dead\-letter queues, you can use CloudWatch metrics and alarms\. For example, when you expect the dead\-letter queue to be always empty, you can create a CloudWatch alarm for the `NumberOfMessagesSent` metric\. You can set the alarm threshold to 0 and specify an Amazon SNS topic to be notified when the alarm goes off\. This Amazon SNS topic can deliver your alarm notification to any endpoint type \(such as an email address, phone number, or mobile pager app\)\.

You can use CloudWatch Logs to investigate the exceptions that cause any Amazon SNS deliveries to fail and for messages to be sent to dead\-letter queues\. Amazon SNS can log both successful and failed deliveries in CloudWatch\. For more information, see [Amazon SNS Message Delivery Status](sns-topic-attributes.md)\.