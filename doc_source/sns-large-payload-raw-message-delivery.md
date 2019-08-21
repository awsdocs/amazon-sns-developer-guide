# Amazon SNS Large Payload and Raw Message Delivery<a name="sns-large-payload-raw-message-delivery"></a>

Amazon SNS \(and Amazon SQS\) allows you to send large payload messages \(from 64 to 256 kilobytes in size\)\. To send large payloads, you must use an AWS SDK that supports Signature Version 4\.

In addition to sending large payloads, with Amazon SNS you can now enable raw message delivery for messages delivered to either Amazon SQS endpoints or HTTP/S endpoints\. This eliminates the need for the endpoints to process JSON formatting, which is created for the Amazon SNS metadata when raw message delivery is not selected\. For example when enabling raw message delivery for an Amazon SQS endpoint, the Amazon SNS metadata is not included and the published message is delivered to the subscribed Amazon SQS endpoint as is\. When enabling raw message delivery for HTTP/S endpoints, the messages will contain an additional HTTP header `x-amz-sns-rawdelivery` with a value of `true` to indicate that the message is being published raw instead of with JSON formatting\. This enables those endpoints to understand what is being delivered and enables easier transition for subscriptions from JSON to raw delivery\. 

To enable raw message delivery using one of the AWS SDKs, you must use the `SetSubscriptionAttribute` action and configure the `RawMessageDelivery` attribute with a value of `true`\. The default value is `false`\. 

## Enabling Raw Message Delivery Using the AWS Management Console<a name="raw-message-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic subscribed an Amazon SQS or HTTP/S endpoint\.

1. On the ***MyTopic*** page, in the **Subscription** section, choose a subscription and choose **Edit**\.

1. On the **Edit *EXAMPLE1\-23bc\-4567\-d890\-ef12g3hij456*** page, in the **Details** section, choose **Enable raw message delivery**\.

1. Choose **Save changes**\.