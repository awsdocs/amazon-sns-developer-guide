# Message ordering details for FIFO topics<a name="fifo-topic-message-ordering"></a>

An Amazon SNS FIFO topic delivers messages to subscribed Amazon SQS FIFO queues in the exact order that the messages are published to the topic\. With an SQS FIFO queue, the queues' consumers receive the messages in the exact order that the messages are sent to the queue\. This setup preserves end\-to\-end message ordering, as shown in the following example based on the [FIFO topics example use case](fifo-example-use-case.md)\.

![\[Strictly ordered message delivery in an auto parts ecommerce platform.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-ordering-1.png)

Note that there is no implied ordering of the subscribers\. The following example shows that message **m1** is delivered first to the wholesale subscriber and then to the retail subscriber\. Message **m2** is delivered first to the retail subscriber and then to the wholesale subscriber\. Though the two messages are delivered to the subscribers in a different order, message ordering is preserved for each subscriber\. Each subscriber is perceived in isolation from any other subscribers\.

![\[Strictly ordered message delivery for each subscriber.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-ordering-2.png)

If an SQS FIFO queue subscriber becomes unreachable, it can get out of sync\. For example, say the wholesale application queue owner mistakenly changes the [ Amazon SQS queue policy](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-using-identity-based-policies.html) in a way that prevents the Amazon SNS service principal from delivering messages to the queue\. In this case, wholesale price updates aren't delivered, but retail price updates succeed, causing the subscribers to be out of sync\. When the wholesale application queue owner corrects the queue policy, Amazon SNS resumes delivering messages to the subscribed queue\. Any messages that were published to the topic while the queue was incorrectly configured are dropped, unless the subscription has a [dead\-letter queue](sns-dead-letter-queues.md) configured\.

![\[The wholesale queue subscriber becomes temporarily unreachable.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-ordering-3.png)

You can have multiple applications \(or multiple threads within the same application\) publishing messages to an SNS FIFO topic in parallel\. When you do this, you effectively delegate message sequencing to the Amazon SNS service\. To determine the established sequence of messages, you can check the sequence number\.

The sequence number is a large, non\-consecutive, ever\-increasing number that Amazon SNS assigns to each message that you publish\. The sequence number is passed to the subscribed SQS FIFO queues as part of the message body\. However, if you enable [raw message delivery](sns-large-payload-raw-message-delivery.md), the message that's delivered to the SQS FIFO queue doesn't include the sequence number or any other SNS message metadata\.

![\[Amazon SNS assigns a unique sequence number to each message and passes the sequence number to Amazon SQS.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-ordering-4.png)

Amazon SNS FIFO topics define ordering in the context of a message group\. For more information, see [Message grouping for FIFO topics](fifo-message-grouping.md)\.