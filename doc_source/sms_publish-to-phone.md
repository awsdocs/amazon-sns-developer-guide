# Sending an SMS message<a name="sms_publish-to-phone"></a>

You can use Amazon SNS to send SMS messages to SMS\-enabled devices\. You can publish messages directly to the phone numbers for these devices, and you do not need to subscribe the phone numbers to an Amazon SNS topic\. 

Subscribing phone numbers to a topic can be still useful if you want to publish each message to multiple phone numbers at once\. For steps on how to publish an SMS message to a topic, see [Sending an SMS message to multiple phone numbers](sms_publish-to-topic.md)\.

When you send a message, you can control whether the message is optimized for cost or reliable delivery, and you can specify a sender ID\. If you send the message programmatically using the Amazon SNS API or AWS SDKs, you can specify a maximum price for the message delivery\.

Each SMS message can contain up to 140 bytes, and the character quota depends on the encoding scheme\. For example, an SMS message can contain:
+ 160 GSM characters
+ 140 ASCII characters
+ 70 UCS\-2 characters

If you publish a message that exceeds the size quota, Amazon SNS sends it as multiple messages, each fitting within the size quota\. Messages are not cut off in the middle of a word but on whole\-word boundaries\. The total size quota for a single SMS publish action is 1600 bytes\.

When you send an SMS message, specify the phone number using the E\.164 format\. E\.164 is a standard for the phone number structure used for international telecommunication\. Phone numbers that follow this format can have a maximum of 15 digits, and they are prefixed with the plus character \(\+\) and the country code\. For example, a U\.S\. phone number in E\.164 format would appear as \+1XXX5550100\.

**Topics**
+ [Sending a message \(console\)](#sms_publish_console)
+ [Sending a message \(AWS SDKs\)](#sms_publish_sdk)

## Sending a message \(console\)<a name="sms_publish_console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, set the region selector to a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Mobile Text messaging \(SMS\)** page, choose **Publish text message**\. The **Publish SMS message** window opens\.

1. For **Message type**, choose one of the following:
   + **Promotional** – Noncritical messages, such as marketing messages\. Amazon SNS optimizes the message delivery to incur the lowest cost\.
   + **Transactional** – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\. Amazon SNS optimizes the message delivery to achieve the highest reliability\.

   This message\-level setting overrides your default message type, which you set on the **Text messaging preferences** page\.

   For pricing information for promotional and transactional messages, see [Global SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\.

1. For **Number**, type the phone number to which you want to send the message\.

1. For **Message**, type the message to send\.

1. \(Optional\) For **Sender ID**, type a custom ID that contains 3\-11 alphanumeric characters, including at least one letter and no spaces\. The sender ID is displayed as the message sender on the receiving device\. For example, you can use your business brand to make the message source easier to recognize\.

   Support for sender IDs varies by country and/or region\. For example, messages delivered to U\.S\. phone numbers will not display the sender ID\. For the countries and regions that support sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

   If you do not specify a sender ID, the message will display a long code as the sender ID in supported countries or regions\. For countries and regions that require an alphabetic sender ID, the message displays *NOTICE* as the sender ID\. 

   This message\-level sender ID overrides your default sender ID, which you set on the **Text messaging preferences** page\.

1. Choose **Send text message**\.

## Sending a message \(AWS SDKs\)<a name="sms_publish_sdk"></a>

To send an SMS message using one of the AWS SDKs, use the action in that SDK that corresponds to the `Publish` request in the Amazon SNS API\. With this request, you can send an SMS message directly to a phone number\. You can also use the `MessageAttributes` parameter to set values for the following attribute names:

`AWS.SNS.SMS.SenderID`  
A custom ID that contains 3\-11 alphanumeric characters, including at least one letter and no spaces\. The sender ID is displayed as the message sender on the receiving device\. For example, you can use your business brand to make the message source easier to recognize\.  
Support for sender IDs varies by country and/or region\. For example, messages delivered to U\.S\. phone numbers will not display the sender ID\. For the countries and regions that support sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.  
If you do not specify a sender ID, the message will display a long code as the sender ID in supported countries and regions\. For countries or regions that require an alphabetic sender ID, the message displays *NOTICE* as the sender ID\.  
This message\-level attribute overrides the account\-level attribute `DefaultSenderID`, which you set using the `SetSMSAttributes` request\.

`AWS.SNS.SMS.MaxPrice`  
The maximum amount in USD that you are willing to spend to send the SMS message\. Amazon SNS will not send the message if it determines that doing so would incur a cost that exceeds the maximum price\.  
This attribute has no effect if your month\-to\-date SMS costs have already exceeded the quota set for the `MonthlySpendLimit` attribute, which you set using the `SetSMSAttributes` request\.  
If you are sending the message to an Amazon SNS topic, the maximum price applies to each message delivery to each phone number that is subscribed to the topic\.

`AWS.SNS.SMS.SMSType`  
The type of message that you are sending:  
+ `Promotional` \(default\) – Noncritical messages, such as marketing messages\. Amazon SNS optimizes the message delivery to incur the lowest cost\.
+ `Transactional` – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\. Amazon SNS optimizes the message delivery to achieve the highest reliability\.
This message\-level attribute overrides the account\-level attribute `DefaultSMSType`, which you set using the `SetSMSAttributes` request\.

### \(Optional\) Setting message attributes<a name="sms_attributes_sdks"></a>

The following examples show how to set message attributes using the Amazon SNS clients that are provided by the AWS SDKs\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [AWS SDK for \.NET Developer Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) or [AWS SDK for Java V2 Developer Guide](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html)

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you set message attribute values by constructing a map that associates the attribute keys with `MessageAttributeValue` objects\. Each `MessageAttributeValue` object is initialized with an attribute value, and each object declares the data type for the value\. The following example sets the sender ID to "mySenderID", maximum price to 0\.50 USD, and SMS type to promotional:

```
Map<String, MessageAttributeValue> smsAttributes =
        new HashMap<String, MessageAttributeValue>();
smsAttributes.put("AWS.SNS.SMS.SenderID", new MessageAttributeValue()
        .withStringValue("mySenderID") //The sender ID shown on the device.
        .withDataType("String"));
smsAttributes.put("AWS.SNS.SMS.MaxPrice", new MessageAttributeValue()
        .withStringValue("0.50") //Sets the max price to 0.50 USD.
        .withDataType("Number"));
smsAttributes.put("AWS.SNS.SMS.SMSType", new MessageAttributeValue()
        .withStringValue("Promotional") //Sets the type to promotional.
        .withDataType("String"));
```

When you send an SMS message, you will apply your attributes to the `PublishRequest` object\.

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET\. you set message attribute values by constructing a map that associates the attribute keys with `MessageAttributeValue` objects\. Each `MessageAttributeValue` object is initialized with an attribute value, and each object declares the data type for the value\. The following example sets the sender ID to "mySenderID", maximum price to 0\.50 USD, and SMS type to promotional:

```
PublishRequest pubRequest = new PublishRequest();
// add optional MessageAttributes...
pubRequest.MessageAttributes["AWS.SNS.SMS.SenderID"] = 
    new MessageAttributeValue{ StringValue = "mySenderId", DataType = "String"};
pubRequest.MessageAttributes["AWS.SNS.SMS.MaxPrice"] =
    new MessageAttributeValue { StringValue = "0.50", DataType = "Number" };
pubRequest.MessageAttributes["AWS.SNS.SMS.SMSType"] =
    new MessageAttributeValue { StringValue = "Promotional", DataType = "String" };
```

------

### Sending a message<a name="sms_publish_sdks"></a>

The following examples show how to send a message using the Amazon SNS clients that are provided by the AWS SDKs\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [AWS SDK for \.NET Developer Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) or [AWS SDK for Java V2 Developer Guide](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html)

------
#### [ AWS SDK for Java ]

The following example uses the `publish` method of the `AmazonSNSClient` class in the AWS SDK for Java\. This example sends a message directly to a phone number:

```
public static void main(String[] args) {
        AmazonSNSClient snsClient = new AmazonSNSClient();
        String message = "My SMS message";
        String phoneNumber = "+1XXX5550100";
        Map<String, MessageAttributeValue> smsAttributes = 
                new HashMap<String, MessageAttributeValue>();
        //<set SMS attributes>
        sendSMSMessage(snsClient, message, phoneNumber, smsAttributes);
}

public static void sendSMSMessage(AmazonSNSClient snsClient, String message, 
		String phoneNumber, Map<String, MessageAttributeValue> smsAttributes) {
        PublishResult result = snsClient.publish(new PublishRequest()
                        .withMessage(message)
                        .withPhoneNumber(phoneNumber)
                        .withMessageAttributes(smsAttributes));
        System.out.println(result); // Prints the message ID.
}
```

When you run this example, the message ID is displayed in the console output window of your IDE:

```
{MessageId: 9b888f80-15f7-5c30-81a2-c4511a3f5229}
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `Publish` method of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET\. This example sends a message directly to a phone number:

```
static void Main(string[] args)
{
    AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
    PublishRequest pubRequest = new PublishRequest();
    pubRequest.Message = "My SMS message";
    pubRequest.PhoneNumber = "+1XXX5550100";
    // add optional MessageAttributes, for example:
    //   pubRequest.MessageAttributes.Add("AWS.SNS.SMS.SenderID", new MessageAttributeValue
    //      { StringValue = "SenderId", DataType = "String" });
    PublishResponse pubResponse = snsClient.Publish(pubRequest);
    Console.WriteLine(pubResponse.MessageId);
}
```

When you run this example, the message ID is displayed in the console output window of your IDE:

```
9b888f80-15f7-5c30-81a2-c4511a3f5229
```

------