# Publishing to a topic<a name="sms_publish-to-topic"></a>

You can publish a single SMS message to many phone numbers at once by subscribing those phone numbers to an Amazon SNS topic\. An SNS topic is a communication channel to which you can add subscribers and then publish messages to all of those subscribers\. A subscriber receives all messages published to the topic until you cancel the subscription, or until the subscriber opts out of receiving SMS messages from your AWS account\.

**Topics**
+ [Sending a message to a topic \(console\)](#sms_publish-to-topic_console)
+ [Sending a message to a topic \(AWS SDKs\)](#sms_publish-to-topic_sdk)

## Sending a message to a topic \(console\)<a name="sms_publish-to-topic_console"></a>

**To create a topic**

Complete the following steps if you don't already have a topic to which you want to send SMS messages\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, choose an [AWS Region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. In the navigation pane, choose **Topics**\.

1. On the **Topics** page, choose **Create topic**\.

1. On the **Create topic** page, under **Details**, do the following:

   1. For **Type**, choose **Standard**\.

   1. For **Name**, enter a topic name\.

   1. \(Optional\) For **Display name**, enter a custom prefix for your SMS messages\. When you send a message to the topic, Amazon SNS prepends the display name followed by a right angle bracket \(>\) and a space\. Display names are not case sensitive, and Amazon SNS converts display names to uppercase characters\. For example, if the display name of a topic is `MyTopic` and the message is `Hello World!`, the message appears as:

      ```
      MYTOPIC> Hello World!
      ```

1. Choose **Create topic**\. The topic's name and Amazon Resource Name \(ARN\) appear on the **Topics** page\.

**To create an SMS subscription**

You can use subscriptions to send an SMS message to multiple recipients by publishing the message only once to your topic\.
**Note**  
When you start using Amazon SNS to send SMS messages, your AWS account is in the *SMS sandbox*\. The SMS sandbox provides a safe environment for you to try Amazon SNS features without risking your reputation as an SMS sender\. While your account is in the SMS sandbox, you can use all of the features of Amazon SNS, but you can send SMS messages only to verified destination phone numbers\. For more information, see [SMS sandbox](sns-sms-sandbox.md)\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, under **Details**, do the following:

   1. For **Topic ARN**, enter or choose the Amazon Resource Name \(ARN\) of the topic to which you want to send SMS messages\.

   1. For **Protocol**, choose **SMS**\.

   1. For **Endpoint**, enter the phone number that you want to subscribe to your topic\.

1. Choose **Create subscription**\. The subscription information appears on the **Subscriptions** page\.

   To add more phone numbers, repeat these steps\. You can also add other types of subscriptions, such as email\.

**To send a message**

When you publish a message to a topic, Amazon SNS attempts to deliver that message to every phone number that is subscribed to the topic\.

1. In the [Amazon SNS console](https://console.aws.amazon.com/sns/home), on the **Topics** page, choose the name of the topic to which you want to send SMS messages\.

1. On the topic details page, choose **Publish message**\.

1. On the **Publish message to topic** page, under **Message details**, do the following:

   1. For **Subject**, keep the field blank unless your topic contains email subscriptions and you want to publish to both email and SMS subscriptions\. Amazon SNS uses the **Subject** that you enter as the email subject line\.

   1. \(Optional\) For **Time to Live \(TTL\)**, enter a number of seconds that Amazon SNS has to send your SMS message to any mobile application endpoint subscribers\.

1. Under **Message body**, do the following:

   1. For **Message structure**, choose **Identical payload for all delivery protocols** to send the same message to all protocol types subscribed to your topic\. Or, choose **Custom payload for each delivery protocol** to customize the message for subscribers of different protocol types\. For example, you can enter a default message for phone number subscribers and a custom message for email subscribers\.

   1. For **Message body to send to the endpoint**, enter your message, or your custom messages per delivery protocol\.

      If your topic has a display name, Amazon SNS adds it to the message, which increases the message length\. The display name length is the number of characters in the name plus two characters for the right angle bracket \(>\) and the space that Amazon SNS adds\.

      For information about the size quotas for SMS messages, see [Publishing to a mobile phone](sms_publish-to-phone.md)\.

1. \(Optional\) For **Message attributes**, add message metadata such as timestamps, signatures, and IDs\.

1. Choose **Publish message**\. Amazon SNS sends the SMS message and displays a success message\.

## Sending a message to a topic \(AWS SDKs\)<a name="sms_publish-to-topic_sdk"></a>

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

The following code example shows how to:
+ Create an Amazon SNS topic\.
+ Subscribe phone numbers to the topic\.
+ Publish SMS messages to the topic so that all subscribed phone numbers receive the message at once\.

------
#### [ Java ]

**SDK for Java 1\.x**  
Create a topic and return its ARN\.  

```
public static String createSNSTopic(AmazonSNSClient snsClient) {
    CreateTopicRequest createTopic = new CreateTopicRequest("mySNSTopic");
    CreateTopicResult result = snsClient.createTopic(createTopic);
    System.out.println("Create topic request: " +
        snsClient.getCachedResponseMetadata(createTopic));
    System.out.println("Create topic result: " + result);
    return result.getTopicArn();
}
```
Subscribe an endpoint to a topic\.  

```
public static void subscribeToTopic(AmazonSNSClient snsClient, String topicArn,
        String protocol, String endpoint) {
    SubscribeRequest subscribe = new SubscribeRequest(topicArn, protocol, endpoint);
    SubscribeResult subscribeResult = snsClient.subscribe(subscribe);
    System.out.println("Subscribe request: " +
        snsClient.getCachedResponseMetadata(subscribe));
    System.out.println("Subscribe result: " + subscribeResult);
}
```
Set attributes on the message, such as the ID of the sender, the maximum price, and its type\. Message attributes are optional\.  

```
public static void addMessageAttributes(Map<String, MessageAttributeValue> smsAttributes) {
    smsAttributes.put("AWS.SNS.SMS.SenderID", new MessageAttributeValue()
        .withStringValue("mySenderID") //The sender ID shown on the device.
        .withDataType("String"));
    smsAttributes.put("AWS.SNS.SMS.MaxPrice", new MessageAttributeValue()
        .withStringValue("0.50") //Sets the max price to 0.50 USD.
        .withDataType("Number"));
    smsAttributes.put("AWS.SNS.SMS.SMSType", new MessageAttributeValue()
        .withStringValue("Promotional") //Sets the type to promotional.
        .withDataType("String"));
}
```
Publish a message to a topic\. The message is sent to every subscriber\.  

```
public static void sendSMSMessageToTopic(AmazonSNSClient snsClient, String topicArn,
        String message, Map<String, MessageAttributeValue> smsAttributes) {
    PublishResult result = snsClient.publish(new PublishRequest()
        .withTopicArn(topicArn)
        .withMessage(message)
        .withMessageAttributes(smsAttributes));
    System.out.println(result);
}
```
Call the previous functions to create a topic, subscribe a phone number, set message attributes, and publish a message to the topic\.  

```
public static void main(String[] args) {
    AmazonSNSClient snsClient = new AmazonSNSClient();

    String topicArn = createSNSTopic(snsClient);
    String phoneNumber = "+1XXX5550100";
    // Specify a protocol of "sms" when subscribing a phone number.
    subscribeToTopic(snsClient, topicArn, "sms", phoneNumber);

    String message = "My SMS message";
    Map<String, MessageAttributeValue> smsAttributes =
        new HashMap<String, MessageAttributeValue>();
    addMessageAttributes(smsAttributes)
    sendSMSMessageToTopic(snsClient, topicArn, message, smsAttributes);
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/java/example_code/sns#code-examples)\. 

------