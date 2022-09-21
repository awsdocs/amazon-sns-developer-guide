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

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
Create a topic and return its ARN\.  

```
    public static String createSNSTopic(SnsClient snsClient, String topicName ) {

        CreateTopicResponse result = null;
        try {
            CreateTopicRequest request = CreateTopicRequest.builder()
                .name(topicName)
                .build();

            result = snsClient.createTopic(request);
            return result.topicArn();

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```
Subscribe an endpoint to a topic\.  

```
    public static void subTextSNS( SnsClient snsClient, String topicArn, String phoneNumber) {

        try {
            SubscribeRequest request = SubscribeRequest.builder()
                .protocol("sms")
                .endpoint(phoneNumber)
                .returnSubscriptionArn(true)
                .topicArn(topicArn)
                .build();

            SubscribeResponse result = snsClient.subscribe(request);
            System.out.println("Subscription ARN: " + result.subscriptionArn() + "\n\n Status is " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
Set attributes on the message, such as the ID of the sender, the maximum price, and its type\. Message attributes are optional\.  

```
public class SetSMSAttributes {
    public static void main(String[] args) {

        HashMap<String, String> attributes = new HashMap<>(1);
        attributes.put("DefaultSMSType", "Transactional");
        attributes.put("UsageReportS3Bucket", "janbucket" );

        SnsClient snsClient = SnsClient.builder()
            .region(Region.US_EAST_1)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();
        setSNSAttributes(snsClient, attributes);
        snsClient.close();
    }

    public static void setSNSAttributes( SnsClient snsClient, HashMap<String, String> attributes) {

        try {
            SetSmsAttributesRequest request = SetSmsAttributesRequest.builder()
                .attributes(attributes)
                .build();

            SetSmsAttributesResponse result = snsClient.setSMSAttributes(request);
            System.out.println("Set default Attributes to " + attributes + ". Status was " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
Publish a message to a topic\. The message is sent to every subscriber\.  

```
    public static void pubTextSMS(SnsClient snsClient, String message, String phoneNumber) {
        try {
            PublishRequest request = PublishRequest.builder()
                .message(message)
                .phoneNumber(phoneNumber)
                .build();

            PublishResponse result = snsClient.publish(request);
            System.out.println(result.messageId() + " Message sent. Status was " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```

------