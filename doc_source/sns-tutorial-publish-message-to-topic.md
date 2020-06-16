# Tutorial: Publishing a message to an Amazon SNS topic<a name="sns-tutorial-publish-message-to-topic"></a>

After you [create a topic](sns-tutorial-create-topic.md) and [subscribe an endpoint to it](sns-tutorial-create-subscribe-endpoint-to-topic.md), you can *publish* messages to a topic\. When the message is published, Amazon SNS attempts to deliver the message to every endpoint \(such as AWS Lambda, Amazon SQS, HTTP/S, or an email address\) subscribed to the topic\.

The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to publish a message to a topic\.

**Important**  
You can publish messages only to topics and endpoints in the same AWS Region\.

**Topics**
+ [AWS Management Console](#publish-message-to-topic-aws-console)
+ [AWS SDK for Java](#publish-message-to-topic-aws-java)
+ [AWS SDK for \.NET](#publish-message-to-topic-aws-dot-net)

## To publish a message to an Amazon SNS topic using the AWS Management Console<a name="publish-message-to-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose the topic you created earlier and then choose **Publish message**\.

1. On the **Publish message to topic** page, do the following:

   1. \(Optional\) In the **Message details** section, enter the **Subject**, for example:

      ```
      Hello from Amazon SNS!
      ```

   1. In the **Message body** section, do one of the following:
      + Choose **Identical payload for all delivery protocols** and then enter the message, for example:

        ```
        Publishing a message to an Amazon SNS topic.
        ```
      + Choose **Custom payload for each delivery protocol** and then use a JSON object to define the message to send to each protocol, for example:

        ```
        {
          "default": "Sample fallback message",
          "email": "Sample message for email endpoints",
          "sqs": "Sample message for Amazon SQS endpoints",
          "lambda": "Sample message for AWS Lambda endpoints",
          "http": "Sample message for HTTP endpoints",
          "https": "Sample message for HTTPS endpoints",
          "sms": "Sample message for SMS endpoints",
          "APNS": "{\"aps\":{\"alert\": \"Sample message for iOS endpoints\"} }",
          "APNS_SANDBOX": "{\"aps\":{\"alert\":\"Sample message for iOS development endpoints\"}}",
          "APNS_VOIP": "{\"aps\":{\"alert\":\"Sample message for Apple VoIP endpoints\"}}",
          "APNS_VOIP_SANDBOX": "{\"aps\":{\"alert\": \"Sample message for Apple VoIP development endpoints\"} }",
          "MACOS": "{\"aps\":{\"alert\":\"Sample message for MacOS endpoints\"}}",
          "MACOS_SANDBOX": "{\"aps\":{\"alert\": \"Sample message for MacOS development endpoints\"} }",
          "GCM": "{ \"data\": { \"message\": \"Sample message for Android endpoints\" } }",
          "ADM": "{ \"data\": { \"message\": \"Sample message for FireOS endpoints\" } }",
          "BAIDU": "{\"title\":\"Sample message title\",\"description\":\"Sample message for Baidu endpoints\"}",
          "MPNS": "<?xml version=\"1.0\" encoding=\"utf-8\"?><wp:Notification xmlns:wp=\"WPNotification\"><wp:Tile><wp:Count>ENTER COUNT</wp:Count><wp:Title>Sample message for Windows Phone 7+ endpoints</wp:Title></wp:Tile></wp:Notification>",
          "WNS": "<badge version=\"1\" value=\"42\"/>"
        }
        ```

        For more information, see [Send custom platform\-specific payloads to mobile devices](sns-send-custom-platform-specific-payloads-mobile-devices.md)\.

   1. In the **Message attributes** section, add any attributes that you want Amazon SNS to match with the subscription attribute `FilterPolicy` to decide whether the subscribed endpoint is interested in the published message\.

      1. Select an attribute **Type**, for example **String\.Array**\.
**Note**  
If the attribute type is **String\.Array**, enclose the array in square brackets \(`[]`\)\. Within the array, enclose string values in double quotation marks\. You don't need quotation marks for numbers or for the keywords `true`, `false`, and `null`\.

      1. Enter a **Name** for the attribute, for example `customer_interests`\.

      1. Enter a **Value** for the attribute, for example `["soccer", "rugby", "hockey"]`\.

      If the attribute type is **String**, **String\.Array**, or **Number**, Amazon SNS evaluates the message attribute against a subscription's filter policy \(if present\) before sending the message to the subscription\.

      For more information, see [Amazon SNS message attributes](sns-message-attributes.md)\.

   1. Choose **Publish message**\.

      The message is published to the topic and the ***MyTopic*** page is displayed\.

      The topic's **Name**, **ARN**, \(optional\) **Display name**, and **Topic owner**'s AWS account ID are displayed in the **Details** section\.

1. In your email client, check the email address that you specified earlier and read the email from Amazon SNS\.

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