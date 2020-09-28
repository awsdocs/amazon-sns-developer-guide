# Message publishing<a name="sns-publishing"></a>

After you [create an Amazon SNS topic](sns-create-topic.md) and [subscribe](sns-create-subscribe-endpoint-to-topic.md) an endpoint to it, you can *publish* messages to the topic\. When a message is published, Amazon SNS attempts to deliver the message to the subscribed endpoints\. An endpoint can be an AWS Lambda function, an Amazon Simple Queue Service \(Amazon SQS\) queue, an HTTP\(S\) endpoint, or an email address\.

**Important**  
You can publish messages only to topics and endpoints in the same AWS Region\.

**Topics**
+ [To publish messages to Amazon SNS topics using the AWS Management Console](#sns-publishing-messages)
+ [To publish a message to an Amazon SNS topic using the AWS SDK for Java](#publish-message-to-topic-aws-java)
+ [To publish a message to an Amazon SNS topic using the AWS SDK for \.NET](#publish-message-to-topic-aws-dot-net)
+ [Amazon SNS message attributes](sns-message-attributes.md)
+ [Publishing large messages with Amazon SNS and Amazon S3](large-message-payloads.md)

## To publish messages to Amazon SNS topics using the AWS Management Console<a name="sns-publishing-messages"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Topics**\.

1. On the **Topics** page, select a topic, and then choose **Publish message**\.

   The console opens the **Publish message to topic** page\.

1. In the **Message details** section, do the following:

   1. \(Optional\) Enter a message **Subject**\.

   1. \(Optional\) For [mobile push notifications](sns-ttl.md), enter a **Time to Live \(TTL\)** value in seconds\. This is the amount of time that a push notification service—such as Apple Push Notification Service \(APNs\) or Firebase Cloud Messaging \(FCM\)—has to deliver the message to the endpoint\.

1. In the **Message body** section, do one of the following:

   1. Choose **Identical payload for all delivery protocols**, and then enter a message\.

   1. Choose **Custom payload for each delivery protocol**, and then enter a JSON object to define the message to send for each delivery protocol\.

      For more information, see [Publishing with platform\-specific payload](sns-send-custom-platform-specific-payloads-mobile-devices.md)\.

1. In the **Message attributes** section, add any attributes that you want Amazon SNS to match with the subscription attribute `FilterPolicy` to decide whether the subscribed endpoint is interested in the published message\.

   1. For **Type**, choose an attribute type, such as **String\.Array**\.
**Note**  
For attribute type **String\.Array**, enclose the array in square brackets \(`[]`\)\. Within the array, enclose string values in double quotation marks\. You don't need quotation marks for numbers or for the keywords `true`, `false`, and `null`\.

   1. Enter an attribute **Name**, such as `customer_interests`\.

   1. Enter an attribute **Value**, such as `["soccer", "rugby", "hockey"]`\.

   If the attribute type is **String**, **String\.Array**, or **Number**, Amazon SNS evaluates the message attribute against a subscription's [filter policy](sns-message-filtering.md) \(if present\) before sending the message to the subscription\.

   For more information, see [Amazon SNS message attributes](sns-message-attributes.md)\.

1. Choose **Publish message**\.

   The message is published to the topic, and the console opens the topic's **Details** page\.

## To publish a message to an Amazon SNS topic using the AWS SDK for Java<a name="publish-message-to-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   The following code excerpt publishes a message to a topic and then prints the `MessageId`\.

   ```
   // Publish a message to an Amazon SNS topic.
   final String msg = "If you receive this message, publishing a message to an Amazon SNS topic works.";
   final PublishRequest publishRequest = new PublishRequest(topicArn, msg);
   final PublishResult publishResponse = snsClient.publish(publishRequest);
   
   // Print the MessageId of the message.
   System.out.println("MessageId: " + publishResponse.getMessageId());
   ```

1. Compile and run your code\.

   The message is published and the `MessageId` is printed, for example:

   ```
   MessageId: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```

## To publish a message to an Amazon SNS topic using the AWS SDK for \.NET<a name="publish-message-to-topic-aws-dot-net"></a>

1. Specify your AWS credentials\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

1. Write your code\. For more information, see [Programming with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-programming-techniques.html)\.

   The following code excerpt publishes a message to a topic and then prints the `MessageId`\.

   ```
   // Publish a message to an Amazon SNS topic.
   String msg = "If you receive this message, publishing a message to an Amazon SNS topic works.";
   PublishRequest publishRequest = new PublishRequest(topicArn, msg);
   PublishResponse publishResponse = snsClient.Publish(publishRequest);
   
   // Print the MessageId of the published message.
   Console.WriteLine("MessageId: " + publishResponse.MessageId);
   ```

1. Compile and run your code\.

   The message is published and the `MessageId` is printed, for example:

   ```
   MessageId: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```