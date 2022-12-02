# Subscribing to an Amazon SNS topic<a name="sns-create-subscribe-endpoint-to-topic"></a>

To receive messages published to [a topic](sns-create-topic.md), you must *subscribe* an [endpoint](#sns-endpoints) to the topic\. When you subscribe an endpoint to a topic, the endpoint begins to receive messages published to the associated topic\.

**Note**  
HTTP\(S\) endpoints, email addresses, and AWS resources in other AWS accounts require confirmation of the subscription before they can receive messages\.

## To subscribe an endpoint to an Amazon SNS topic<a name="subscribe-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, choose the Amazon Resource Name \(ARN\) of a topic\.

   1. For **Protocol**, choose an endpoint type\.  The available endpoint types are:
      + [**HTTP/HTTPS**](sns-http-https-endpoint-as-subscriber.md)
      + [**Email/Email\-JSON**](sns-email-notifications.md)
      + [**Amazon Kinesis Data Firehose**](sns-firehose-as-subscriber.md)
      + [**Amazon SQS**](sns-sqs-as-subscriber.md)
**Note**  
To subscribe to an [SNS FIFO topic](sns-fifo-topics.md), choose this option\.
      + [**AWS Lambda**](sns-lambda-as-subscriber.md)
      + [**Platform application endpoint**](sns-mobile-application-as-subscriber.md)
      + [**SMS**](sns-mobile-phone-number-as-subscriber.md) 

   1. For **Endpoint**, enter the endpoint value, such as an email address or the ARN of an Amazon SQS queue\.

   1. Kinesis Data Firehose endpoints only: For **Subscription role ARN**, specify the ARN of the IAM role that you created for writing to Kinesis Data Firehose delivery streams\. For more information, see [Prerequisites for subscribing Kinesis Data Firehose delivery streams to Amazon SNS topics](prereqs-kinesis-data-firehose.md)\.

   1. \(Optional\) For Kinesis Data Firehose, Amazon SQS, HTTP/S endpoints, you can also enable raw message delivery\. For more information, see [Amazon SNS raw message delivery](sns-large-payload-raw-message-delivery.md)\.

   1. \(Optional\) To configure a filter policy, expand the **Subscription filter policy** section\. For more information, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md)\.

   1. \(Optional\) To enable payload\-based filtering, configure `Filter Policy Scope` to `MessageBody`\. For more information, see [Amazon SNS subscription filter policy scope](sns-message-filtering.md#sns-message-filtering-scope)\.

   1. \(Optional\) To configure a dead\-letter queue for the subscription, expand the **Redrive policy \(dead\-letter queue\)** section\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md)\.

   1. Choose **Create subscription**\.

      The console creates the subscription and opens the subscription's **Details** page\.