# Publishing to a mobile phone<a name="sms_publish-to-phone"></a>

You can use Amazon SNS to send SMS messages directly to a mobile phone without subscribing the phone number to an Amazon SNS topic\.

**Note**  
Subscribing phone numbers to a topic is useful if you want to send one message to multiple phone numbers at once\. For instructions on publishing an SMS message to a topic, see [Publishing to a topic](sms_publish-to-topic.md)\.

When you send a message, you can control whether the message is optimized for cost or reliable delivery\. You can also specify a [sender ID or origination number](channels-sms-originating-identities.md)\. If you send the message programmatically using the Amazon SNS API or the AWS SDKs, you can specify a maximum price for the message delivery\.

Each SMS message can contain up to 140 bytes, and the character quota depends on the encoding scheme\. For example, an SMS message can contain:
+ 160 GSM characters
+ 140 ASCII characters
+ 70 UCS\-2 characters

If you publish a message that exceeds the size quota, Amazon SNS sends it as multiple messages, each fitting within the size quota\. Messages are not cut off in the middle of a word, but instead on whole\-word boundaries\. The total size quota for a single SMS publish action is 1,600 bytes\.

When you send an SMS message, you specify the phone number using the E\.164 format, a standard phone numbering structure used for international telecommunication\. Phone numbers that follow this format can have a maximum of 15 digits along with the prefix of a plus sign \(\+\) and the country code\. For example, a US phone number in E\.164 format appears as \+1XXX5550100\.

**Topics**
+ [Sending a message \(console\)](#sms_publish_console)
+ [Sending a message \(AWS SDKs\)](#sms_publish_sdk)

## Sending a message \(console\)<a name="sms_publish_console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, choose an [AWS Region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. In the navigation pane, choose **Text messaging \(SMS\)**\.

1. On the **Mobile text messaging \(SMS\)** page, choose **Publish text message**\.

1. On the **Publish SMS message** page, for **Message type**, choose one of the following:
   + **Promotional** – Non\-critical messages, such as marketing messages\.
   + **Transactional** – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\.
**Note**  
This message\-level setting overrides your account\-level default message type\. You can set an account\-level default message type from the **Text messaging preferences** section of the **Mobile text messaging \(SMS\)** page\.

   For pricing information for promotional and transactional messages, see [Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\.

1. For **Destination phone number**, enter the phone number to which you want to send the message\.

1. For **Message**, enter the message to send\.

1. \(Optional\) Under **Origination identities**, specify how to identify yourself to your recipients:
   + To specify a **Sender ID**, type a custom ID that contains 3\-11 alphanumeric characters, including at least one letter and no spaces\. The sender ID is displayed as the message sender on the receiving device\. For example, you can use your business brand to make the message source easier to recognize\.

     Support for sender IDs varies by country and/or region\. For example, messages delivered to U\.S\. phone numbers will not display the sender ID\. For the countries and regions that support sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

     If you do not specify a sender ID, one of the following is displayed as the originating identity:
     + In countries that support long codes, the long code is shown\.
     + In countries where only sender IDs are supported, *NOTICE* is shown\.

     This message\-level sender ID overrides your default sender ID, which you set on the **Text messaging preferences** page\.
   + To specify an **Origination number**, enter a string of 5\-14 numbers to display as the sender's phone number on the receiver's device\. This string must match an origination number that is configured in your AWS account for the destination country\. The origination number can be a 10DLC number, toll\-free number, person\-to\-person long code, or short codes\. For more information, see [Origination identities for SMS messages](channels-sms-originating-identities.md)\.

     If you don't specify an origination number, Amazon SNS selects an origination number to use for the SMS text message, based on your AWS account configuration\.

1. If you're sending SMS messages to recipients in India, expand **Country\-specific attributes**, and specify the following attributes:
   + **Entity ID** – The entity ID or principal entity \(PE\) ID for sending SMS messages to recipients in India\. This ID is a unique string of 1–30 characters that the Telecom Regulatory Authority of India \(TRAI\) provides to identify the entity that you registered with the TRAI\.
   + **Template ID** – The template ID for sending SMS messages to recipients in India\. This ID is a unique, TRAI\-provided string of 1–30 characters that identifies the template that you registered with the TRAI\. The template ID must be associated with the sender ID that you specified for the message\.

   For more information on sending SMS messages to recipients in India, see [Special requirements for sending SMS messages to recipients in India](channels-sms-senderid-india.md)\.

1. Choose **Publish message**\.

**Tip**  
To send SMS messages from an origination number, you can also choose **Origination numbers** in the Amazon SNS console navigation panel\. Choose an origination number that includes **SMS** in the **Capabilities** column, and then choose **Publish text message**\.

## Sending a message \(AWS SDKs\)<a name="sms_publish_sdk"></a>

To send an SMS message using one of the AWS SDKs, use the API operation in that SDK that corresponds to the `Publish` request in the Amazon SNS API\. With this request, you can send an SMS message directly to a phone number\. You can also use the `MessageAttributes` parameter to set values for the following attribute names:

`AWS.SNS.SMS.SenderID`  
A custom ID that contains 3–11 alphanumeric characters, including at least one letter and no spaces\. The sender ID appears as the message sender on the receiving device\. For example, you can use your business brand to help make the message source easier to recognize\.  
Support for sender IDs varies by country or region\. For example, messages delivered to US phone numbers don't display the sender ID\. For a list of the countries or regions that support sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.  
If you don't specify a sender ID, a [long code](channels-sms-originating-identities-long-codes.md) appears as the sender ID in supported countries or regions\. For countries or regions that require an alphabetic sender ID, *NOTICE* appears as the sender ID\.  
This message\-level attribute overrides the account\-level attribute `DefaultSenderID`, which you can set using the `SetSMSAttributes` request\.

`AWS.MM.SMS.OriginationNumber`  
A custom string of 5–14 numbers, which can include an optional leading plus sign \(`+`\)\. This string of numbers appears as the sender's phone number on the receiving device\. The string must match an origination number that's configured in your AWS account for the destination country\. The origination number can be a 10DLC number, toll\-free number, person\-to\-person \(P2P\) long code, or short code\. For more information, see [Origination numbers](channels-sms-originating-identities-origination-numbers.md)\.  
If you don't specify an origination number, Amazon SNS chooses an origination number based on your AWS account configuration\.

`AWS.SNS.SMS.MaxPrice`  
The maximum price in USD that you're willing to spend to send the SMS message\. If Amazon SNS determines that sending the message would incur a cost that exceeds your maximum price, it doesn't send the message\.  
This attribute has no effect if your month\-to\-date SMS costs have already exceeded the quota set for the `MonthlySpendLimit` attribute\. You can set the `MonthlySpendLimit` attribute using the `SetSMSAttributes` request\.  
If you're sending the message to an Amazon SNS topic, the maximum price applies to each message delivery to each phone number that is subscribed to the topic\.

`AWS.SNS.SMS.SMSType`  
The type of message that you're sending:  
+ `Promotional` \(default\) – Non\-critical messages, such as marketing messages\.
+ `Transactional` – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\.
This message\-level attribute overrides the account\-level attribute `DefaultSMSType`, which you can set using the `SetSMSAttributes` request\.

`AWS.MM.SMS.EntityId`  
This attribute is required only for sending SMS messages to recipients in India\.  
This is your entity ID or principal entity \(PE\) ID for sending SMS messages to recipients in India\. This ID is a unique string of 1–30 characters that the Telecom Regulatory Authority of India \(TRAI\) provides to identify the entity that you registered with the TRAI\.

`AWS.MM.SMS.TemplateId`  
This attribute is required only for sending SMS messages to recipients in India\.  
This is your template for sending SMS messages to recipients in India\. This ID is a unique, TRAI\-provided string of 1–30 characters that identifies the template that you registered with the TRAI\. The template ID must be associated with the sender ID that you specified for the message\.

### \(Optional\) Setting message attributes<a name="sms_attributes_sdks"></a>

The following examples show how to set message attributes using the Amazon SNS clients that the AWS SDKs provide\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\. Or, see [Using credentials](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html) in the *AWS SDK for Java Developer Guide*\.

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you set message attribute values by constructing a map that associates the attribute keys with `MessageAttributeValue` objects\. Each `MessageAttributeValue` object is initialized with an attribute value, and each object declares the data type for the value\. The following example sets the sender ID to "mySenderID", maximum price to "0\.50" USD, and SMS type to "Promotional":

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

When you send an SMS message, you apply your attributes to the `PublishRequest` object\.

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET, you set message attribute values by constructing a map that associates the attribute keys with `MessageAttributeValue` objects\. Each `MessageAttributeValue` object is initialized with an attribute value, and each object declares the data type for the value\. The following example sets the sender ID to "mySenderID", maximum price to "0\.50" USD, and SMS type to "Promotional":

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

The following examples show how to send a message using the Amazon SNS clients that the AWS SDKs provide\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\. Or, see [Using credentials](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html) in the *AWS SDK for Java Developer Guide*\.

------
#### [ AWS SDK for Java ]

The following example uses the `publish` operation of the `AmazonSNSClient` class in the AWS SDK for Java\. This example sends a message directly to a phone number:

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

When you run this example, the console output window of your IDE displays the message ID:

```
{MessageId: 9b888f80-15f7-5c30-81a2-c4511a3f5229}
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `Publish` operation of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET\. This example sends a message directly to a phone number:

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