# Amazon SNS message and JSON formats<a name="sns-message-and-json-formats"></a>

Amazon SNS uses the following formats\.

**Topics**
+ [HTTP/HTTPS headers](#http-header)
+ [HTTP/HTTPS subscription confirmation JSON format](#http-subscription-confirmation-json)
+ [HTTP/HTTPS notification JSON format](#http-notification-json)
+ [HTTP/HTTPS unsubscribe confirmation JSON format](#http-unsubscribe-confirmation-json)
+ [SetSubscriptionAttributes delivery policy JSON format](#set-sub-attributes-delivery-policy-json)
+ [SetTopicAttributes delivery policy JSON format](#set-topic-attributes-delivery-policy-json)

## HTTP/HTTPS headers<a name="http-header"></a>

When Amazon SNS sends a subscription confirmation, notification, or unsubscribe confirmation message to HTTP/HTTPS endpoints, it sends a POST message with a number of Amazon SNS\-specific header values\. You can use these header values to do things such as identify the type of message without having to parse the JSON message body to read the `Type` value\.

**`x-amz-sns-message-type`**  
The type of message\. The possible values are `SubscriptionConfirmation`, `Notification`, and `UnsubscribeConfirmation`\.

**`x-amz-sns-message-id`**  
A Universally Unique Identifier, unique for each message published\. For a notification that Amazon SNS resends during a retry, the message ID of the original message is used\.

**`x-amz-sns-topic-arn`**  
The Amazon Resource Name \(ARN\) for the topic that this message was published to\.

**`x-amz-sns-subscription-arn`**  
The ARN for the subscription to this endpoint\.

The following HTTP POST header is an example of a header for a Notification message to an HTTP endpoint\.

```
POST / HTTP/1.1
x-amz-sns-message-type: Notification
x-amz-sns-message-id: 165545c9-2a5c-472c-8df2-7ff2be2b3b1b
x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55
Content-Length: 1336
Content-Type: text/plain; charset=UTF-8
Host: myhost.example.com
Connection: Keep-Alive
User-Agent: Amazon Simple Notification Service Agent
```

## HTTP/HTTPS subscription confirmation JSON format<a name="http-subscription-confirmation-json"></a>

After you subscribe an HTTP/HTTPS endpoint, Amazon SNS sends a subscription confirmation message to the HTTP/HTTPS endpoint\. This message contains a `SubscribeURL` value that you must visit to confirm the subscription \(alternatively, you can use the `Token` value with the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html)\)\.

**Note**  
Amazon SNS doesn'tsend notifications to this endpoint until the subscription is confirmed

The subscription confirmation message is a POST message with a message body that contains a JSON document with the following name\-value pairs\.

**`Message`**  
A string that describes the message\. For subscription confirmation, this string looks like this:  

```
You have chosen to subscribe to the topic arn:aws:sns:us-east-2:123456789012:MyTopic.\nTo confirm the subscription, visit the SubscribeURL included in this message.
```

**`MessageId`**  
A Universally Unique Identifier, unique for each message published\. For a message that Amazon SNS resends during a retry, the message ID of the original message is used\.

**`Signature`**  
Base64\-encoded "SHA1withRSA" signature of the Message, MessageId, Type, Timestamp, and TopicArn values\.

**`SignatureVersion`**  
Version of the Amazon SNS signature used\.

**`SigningCertURL`**  
The URL to the certificate that was used to sign the message\.

**`SubscribeURL`**  
The URL that you must visit in order to confirm the subscription\. Alternatively, you can instead use the `Token` with the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) action to confirm the subscription\.

**`Timestamp`**  
The time \(GMT\) when the subscription confirmation was sent\.

**`Token`**  
A value you can use with the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) action to confirm the subscription\. Alternatively, you can simply visit the `SubscribeURL`\.

**`TopicArn`**  
The Amazon Resource Name \(ARN\) for the topic that this endpoint is subscribed to\.

**`Type`**  
The type of message\. For a subscription confirmation, the type is `SubscriptionConfirmation`\.

The following HTTP POST message is an example of a SubscriptionConfirmation message to an HTTP endpoint\.

```
POST / HTTP/1.1
x-amz-sns-message-type: SubscriptionConfirmation
x-amz-sns-message-id: 165545c9-2a5c-472c-8df2-7ff2be2b3b1b
x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
Content-Length: 1336
Content-Type: text/plain; charset=UTF-8
Host: myhost.example.com
Connection: Keep-Alive
User-Agent: Amazon Simple Notification Service Agent

{
  "Type" : "SubscriptionConfirmation",
  "MessageId" : "165545c9-2a5c-472c-8df2-7ff2be2b3b1b",
  "Token" : "2336412f37...",
  "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
  "Message" : "You have chosen to subscribe to the topic arn:aws:sns:us-west-2:123456789012:MyTopic.\nTo confirm the subscription, visit the SubscribeURL included in this message.",
  "SubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37...",
  "Timestamp" : "2012-04-26T20:45:04.751Z",
  "SignatureVersion" : "1",
  "Signature" : "EXAMPLEpH+DcEwjAPg8O9mY8dReBSwksfg2S7WKQcikcNKWLQjwu6A4VbeS0QHVCkhRS7fUQvi2egU3N858fiTDN6bkkOxYDVrY0Ad8L10Hs3zH81mtnPk5uvvolIC1CXGu43obcgFxeL3khZl8IKvO61GWB6jI9b5+gLPoBc1Q=",
  "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem"
}
```

## HTTP/HTTPS notification JSON format<a name="http-notification-json"></a>

When Amazon SNS sends a notification to a subscribed HTTP or HTTPS endpoint, the POST message sent to the endpoint has a message body that contains a JSON document with the following name\-value pairs\.

**`Message`**  
The Message value specified when the notification was published to the topic\.

**`MessageId`**  
A Universally Unique Identifier, unique for each message published\. For a notification that Amazon SNS resends during a retry, the message ID of the original message is used\.

**`Signature`**  
Base64\-encoded `SHA1withRSA` signature of the Message, MessageId, Subject \(if present\), Type, Timestamp, and TopicArn values\.

**`SignatureVersion`**  
Version of the Amazon SNS signature used\.

**`SigningCertURL`**  
The URL to the certificate that was used to sign the message\.

**`Subject`**  
The Subject parameter specified when the notification was published to the topic\.  
This is an optional parameter\. If no Subject was specified, then this name\-value pair does not appear in this JSON document\.

**`Timestamp`**  
The time \(GMT\) when the notification was published\.

**`TopicArn`**  
The Amazon Resource Name \(ARN\) for the topic that this message was published to\.

**`Type`**  
The type of message\. For a notification, the type is `Notification`\.

**`UnsubscribeURL`**  
A URL that you can use to unsubscribe the endpoint from this topic\. If you visit this URL, Amazon SNS unsubscribes the endpoint and stops sending notifications to this endpoint\.

The following HTTP POST message is an example of a Notification message to an HTTP endpoint\.

```
POST / HTTP/1.1
x-amz-sns-message-type: Notification
x-amz-sns-message-id: 22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324
x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96
Content-Length: 773
Content-Type: text/plain; charset=UTF-8
Host: myhost.example.com
Connection: Keep-Alive
User-Agent: Amazon Simple Notification Service Agent

{
  "Type" : "Notification",
  "MessageId" : "22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324",
  "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
  "Subject" : "My First Message",
  "Message" : "Hello world!",
  "Timestamp" : "2012-05-02T00:54:06.655Z",
  "SignatureVersion" : "1",
  "Signature" : "EXAMPLEw6JRN...",
  "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
  "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96"
}
```

## HTTP/HTTPS unsubscribe confirmation JSON format<a name="http-unsubscribe-confirmation-json"></a>

After an HTTP/HTTPS endpoint is unsubscribed from a topic, Amazon SNS sends an unsubscribe confirmation message to the endpoint\.

The unsubscribe confirmation message is a POST message with a message body that contains a JSON document with the following name\-value pairs\.

**`Message`**  
A string that describes the message\. For unsubscribe confirmation, this string looks like this:  

```
You have chosen to deactivate subscription arn:aws:sns:us-east-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55.\nTo cancel this operation and restore the subscription, visit the SubscribeURL included in this message.
```

**`MessageId`**  
A Universally Unique Identifier, unique for each message published\. For a message that Amazon SNS resends during a retry, the message ID of the original message is used\.

**`Signature`**  
Base64\-encoded "SHA1withRSA" signature of the Message, MessageId, Type, Timestamp, and TopicArn values\.

**`SignatureVersion`**  
Version of the Amazon SNS signature used\.

**`SigningCertURL`**  
The URL to the certificate that was used to sign the message\.

**`SubscribeURL`**  
The URL that you must visit in order to re\-confirm the subscription\. Alternatively, you can instead use the `Token` with the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) action to re\-confirm the subscription\.

**`Timestamp`**  
The time \(GMT\) when the unsubscribe confirmation was sent\.

**`Token`**  
A value you can use with the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) action to re\-confirm the subscription\. Alternatively, you can simply visit the `SubscribeURL`\.

**`TopicArn`**  
The Amazon Resource Name \(ARN\) for the topic that this endpoint has been unsubscribed from\.

**`Type`**  
The type of message\. For a unsubscribe confirmation, the type is `UnsubscribeConfirmation`\.

The following HTTP POST message is an example of a UnsubscribeConfirmation message to an HTTP endpoint\.

```
POST / HTTP/1.1
x-amz-sns-message-type: UnsubscribeConfirmation
x-amz-sns-message-id: 47138184-6831-46b8-8f7c-afc488602d7d
x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55
Content-Length: 1399
Content-Type: text/plain; charset=UTF-8
Host: myhost.example.com
Connection: Keep-Alive
User-Agent: Amazon Simple Notification Service Agent

{
  "Type" : "UnsubscribeConfirmation",
  "MessageId" : "47138184-6831-46b8-8f7c-afc488602d7d",
  "Token" : "2336412f37...",
  "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
  "Message" : "You have chosen to deactivate subscription arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55.\nTo cancel this operation and restore the subscription, visit the SubscribeURL included in this message.",
  "SubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37fb6...",
  "Timestamp" : "2012-04-26T20:06:41.581Z",
  "SignatureVersion" : "1",
  "Signature" : "EXAMPLEHXgJm...",
  "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem"
}
```

## SetSubscriptionAttributes delivery policy JSON format<a name="set-sub-attributes-delivery-policy-json"></a>

If you send a request to the SetSubscriptionAttributes action and set the AttributeName parameter to a value of `DeliveryPolicy`, the value of the AttributeValue parameter must be a valid JSON object\. For example, the following example sets the delivery policy to 5 total retries\.

```
http://sns.us-east-2.amazonaws.com/
?Action=SetSubscriptionAttributes
&SubscriptionArn=arn%3Aaws%3Asns%3Aus-east-2%3A123456789012%3AMy-Topic%3A80289ba6-0fd4-4079-afb4-ce8c8260f0ca
&AttributeName=DeliveryPolicy
&AttributeValue={"healthyRetryPolicy":{"numRetries":5}}
...
```

Use the following JSON format for the value of the AttributeValue parameter\.

```
{
   "healthyRetryPolicy" : {
      "minDelayTarget" :  int,
      "maxDelayTarget" : int,
      "numRetries" : int,
      "numMaxDelayRetries" : int,
      "backoffFunction" : "linear|arithmetic|geometric|exponential"
   },
   "throttlePolicy" : {
      "maxReceivesPerSecond" : int
   }
}
```

For more information about the SetSubscriptionAttribute action, go to [SetSubscriptionAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html) in the *Amazon Simple Notification Service API Reference*\.

## SetTopicAttributes delivery policy JSON format<a name="set-topic-attributes-delivery-policy-json"></a>

If you send a request to the SetTopicAttributes action and set the AttributeName parameter to a value of `DeliveryPolicy`, the value of the AttributeValue parameter must be a valid JSON object\. For example, the following example sets the delivery policy to 5 total retries\.

```
http://sns.us-east-2.amazonaws.com/
?Action=SetTopicAttributes
&TopicArn=arn%3Aaws%3Asns%3Aus-east-2%3A123456789012%3AMy-Topic
&AttributeName=DeliveryPolicy
&AttributeValue={"http":{"defaultHealthyRetryPolicy":{"numRetries":5}}}
...
```

Use the following JSON format for the value of the AttributeValue parameter\.

```
{
   "http" : {
      "defaultHealthyRetryPolicy" : {
         "minDelayTarget":  int,
         "maxDelayTarget": int,
         "numRetries": int,
         "numMaxDelayRetries": int,
         "backoffFunction": "linear|arithmetic|geometric|exponential"
      },
      "disableSubscriptionOverrides" : Boolean,
      "defaultThrottlePolicy" : {
         "maxReceivesPerSecond" : int
      }
   }
}
```

For more information about the `SetTopicAttribute` action, go to [SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html) in the *Amazon Simple Notification Service API Reference*\.