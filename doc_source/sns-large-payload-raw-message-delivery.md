# Amazon SNS raw message delivery<a name="sns-large-payload-raw-message-delivery"></a>

To avoid having [Amazon Kinesis Data Firehose](sns-firehose-as-subscriber.md), [Amazon SQS](sns-sqs-as-subscriber.md), and [HTTP/S](sns-http-https-endpoint-as-subscriber.md) endpoints process the JSON formatting of messages, Amazon SNS allows raw message delivery:
+ When you enable raw message delivery for an Amazon Kinesis Data Firehose endpoint, Amazon SNS adds JSON metadata to the payload\.
+ When you enable raw message delivery for an Amazon SQS endpoint, any Amazon SNS metadata is stripped from the published message and the message is sent as is\.
+ When you enable raw message delivery for HTTP/S endpoints, the HTTP header `x-amz-sns-rawdelivery` with its value set to `true` is added to the message, indicating that the message has been published without JSON formatting\.

To enable raw message delivery using an AWS SDK, you must use the `SetSubscriptionAttribute` API action and set the value of the `RawMessageDelivery` attribute to `true`\.

## Enabling raw message delivery using the AWS Management Console<a name="raw-message-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic subscribed to an Kinesis Data Firehose, Amazon SQS, or HTTP/S endpoint\.

1. On the ***MyTopic*** page, in the **Subscription** section, choose a subscription and choose **Edit**\.

1. On the **Edit *EXAMPLE1\-23bc\-4567\-d890\-ef12g3hij456*** page, in the **Details** section, choose **Enable raw message delivery**\.

1. Choose **Save changes**\.