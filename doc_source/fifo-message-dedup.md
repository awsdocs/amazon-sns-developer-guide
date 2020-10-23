# Message deduplication for FIFO topics<a name="fifo-message-dedup"></a>

 Amazon SNS FIFO topics and Amazon SQS FIFO queues support message deduplication, which provides exactly\-once message delivery and processing as long as the following conditions are met:
+ The subscribed SQS FIFO queue exists and has permissions that allow the Amazon SNS service principal to deliver messages to the queue\.
+ The SQS FIFO queue consumer processes the message and deletes it from the queue before the visibility timeout expires\.
+ The Amazon SNS subscription topic has no [message filtering](fifo-message-filtering.md)\. When you configure message filtering, SNS FIFO topics support at\-most\-once delivery, as messages can be filtered out based on your subscription filter policies\.
+ There are no network disruptions that prevent acknowledgment of the message delivery\.

**Note**  
Message deduplication applies to an entire SNS FIFO topic, not to an individual [message group](fifo-message-grouping.md)\.

When you publish a message to an SNS FIFO topic, the message must include a deduplication ID\. This ID is included in the message that the SNS FIFO topic delivers to the subscribed SQS FIFO queues\.

If a message with a particular deduplication ID is successfully published to an SNS FIFO topic, any message published with the same deduplication ID, within the five\-minute deduplication interval, is accepted but not delivered\. The SNS FIFO topic continues to track the message deduplication ID, even after the message is delivered to subscribed endpoints\.

If the message body is guaranteed to be unique for each published message, you can enable content\-based deduplication for an Amazon SNS FIFO topic and the subscribed SQS FIFO queues\. Amazon SNS uses the message body to generate a unique hash value to use as the deduplication ID for each message, so you don't need to set a deduplication ID when you send each message\.

**Note**  
Message attributes are not included in the hash calculation\.

In the [auto parts price management example use case](fifo-example-use-case.md), the company must set a universally unique deduplication ID for each price update\. This is because the message body can be identical even when the message attribute is different for wholesale and retail\. However, if the company added the business type \(wholesale or retail\) to the message body alongside the product ID and product price, they could enable content\-based duplication in the SNS FIFO topic and the subscribed SQS FIFO queues\.

![\[With message deduplication, multiple messages with duplicate content are delivered only once.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-dedup.png)

In addition to message ordering and deduplication, SNS FIFO topics support message server\-side encryption \(SSE\) with AWS KMS keys, and message privacy via VPC endpoints with AWS PrivateLink\. For more information, see [Message security for FIFO topics](fifo-message-security.md)\.