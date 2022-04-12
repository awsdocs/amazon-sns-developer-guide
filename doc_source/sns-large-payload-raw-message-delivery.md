# Amazon SNS raw message delivery<a name="sns-large-payload-raw-message-delivery"></a>

To avoid having [Amazon Kinesis Data Firehose](sns-firehose-as-subscriber.md), [Amazon SQS](sns-sqs-as-subscriber.md), and [HTTP/S](sns-http-https-endpoint-as-subscriber.md) endpoints process the JSON formatting of messages, Amazon SNS allows raw message delivery:
+ When you enable raw message delivery for Amazon Kinesis Data Firehose or Amazon SQS endpoints, any Amazon SNS metadata is stripped from the published message and the message is sent as is\.
+ When you enable raw message delivery for HTTP/S endpoints, the HTTP header `x-amz-sns-rawdelivery` with its value set to `true` is added to the message, indicating that the message has been published without JSON formatting\.
+ When you enable raw message delivery for HTTP/S endpoints, the message body, client IP, and the required headers are delivered\. When you specify message attributes, it won't be sent\.
+ When you enable raw message delivery for Kinesis Data Firehose endpoints, the message body is delivered\. When you specify message attributes, it won't be sent\.

To enable raw message delivery using an AWS SDK, you must use the `SetSubscriptionAttribute` API action and set the value of the `RawMessageDelivery` attribute to `true`\.

## Enabling raw message delivery using the AWS Management Console<a name="raw-message-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic subscribed to an Kinesis Data Firehose, Amazon SQS, or HTTP/S endpoint\.

1. On the ***MyTopic*** page, in the **Subscription** section, choose a subscription and choose **Edit**\.

1. On the **Edit *EXAMPLE1\-23bc\-4567\-d890\-ef12g3hij456*** page, in the **Details** section, choose **Enable raw message delivery**\.

1. Choose **Save changes**\.

## Message format examples<a name="raw-message-examples"></a>

In the following examples, the same message is sent to the same Amazon SQS queue twice\. The only difference is that raw message delivery is disabled for the first message, and enabled for the second\. 
+ Raw message delivery is **disabled**

  ```
  {
    "Type": "Notification",
    "MessageId": "dc1e94d9-56c5-5e96-808d-cc7f68faa162",
    "TopicArn": "arn:aws:sns:us-east-2:111122223333:ExampleTopic1",
    "Subject": "TestSubject",
    "Message": "This is a test message.",
    "Timestamp": "2021-02-16T21:41:19.978Z",
    "SignatureVersion": "1",
    "Signature": "FMG5tlZhJNHLHUXvZgtZzlk24FzVa7oX0T4P03neeXw8ZEXZx6z35j2FOTuNYShn2h0bKNC/zLTnMyIxEzmi2X1shOBWsJHkrW2xkR58ABZF+4uWHEE73yDVR4SyYAikP9jstZzDRm+bcVs8+T0yaLiEGLrIIIL4esi1llhIkgErCuy5btPcWXBdio2fpCRD5x9oR6gmE/rd5O7lX1c1uvnv4r1Lkk4pqP2/iUfxFZva1xLSRvgyfm6D9hNklVyPfy+7TalMD0lzmJuOrExtnSIbZew3foxgx8GT+lbZkLd0ZdtdRJlIyPRP44eyq78sU0Eo/LsDr0Iak4ZDpg8dXg==",
    "SigningCertURL": "https://sns.us-east-2.amazonaws.com/SimpleNotificationService-010a507c1833636cd94bdb98bd93083a.pem",
    "UnsubscribeURL": "https://sns.us-east-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-2:111122223333:ExampleTopic1:e1039402-24e7-40a3-a0d4-797da162b297"
  }
  ```
+ Raw message delivery is **enabled**

  ```
  This is a test message.
  ```