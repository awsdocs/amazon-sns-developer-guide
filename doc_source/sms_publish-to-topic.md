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

To send an SMS message to a topic using one of the [AWS SDKs](http://aws.amazon.com/getting-started/tools-sdks/), use the API operations in that SDK that correspond to the following requests in the Amazon SNS API\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\. Or, see [Using credentials](https://docs.aws.amazon.com/sdk-for-java/latest/developer-guide/credentials.html) in the *AWS SDK for Java Developer Guide*\.

`CreateTopic`  
Creates a topic to which you can subscribe phone numbers and then publish messages to all subscribed phone numbers at once\.

`Subscribe`  
Subscribes a phone number to a topic\.

`Publish`  
Sends a message to each phone number subscribed to a topic\.  
You can use the `MessageAttributes` parameter to set several attributes for the message \(for example, the maximum price\)\. For more information, see [Sending a message \(AWS SDKs\)](sms_publish-to-phone.md#sms_publish_sdk)\.

### Creating a topic<a name="sms_create_topic_sdks"></a>

The following examples show how to create a topic using the Amazon SNS clients that the AWS SDKs provide\.

------
#### [ AWS SDK for Java ]

The following example uses the `createTopic` operation of the `AmazonSNSClient` class in the AWS SDK for Java:

```
public static void main(String[] args) {
    AmazonSNSClient snsClient = new AmazonSNSClient();
    String topicArn = createSNSTopic(snsClient);
}

public static String createSNSTopic(AmazonSNSClient snsClient) {
    CreateTopicRequest createTopic = new CreateTopicRequest("mySNSTopic");
    CreateTopicResult result = snsClient.createTopic(createTopic);
    System.out.println("Create topic request: " + 
        snsClient.getCachedResponseMetadata(createTopic));
    System.out.println("Create topic result: " + result);
    return result.getTopicArn();
}
```

The example uses the `getCachedResponseMetadata` operation to get the request ID\.

When you run this example, the console output window of your IDE displays the following:

```
{TopicArn: arn:aws:sns:us-east-1:123456789012:mySNSTopic}
CreateTopicRequest - {AWS_REQUEST_ID=93f7fc90-f131-5ca3-ab18-b741fef918b5}
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `createTopic` operation of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET:

```
static void Main(string[] args)
{
    AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
    String topicArn = CreateSNSTopic(snsClient);
}

public static String CreateSNSTopic(AmazonSimpleNotificationServiceClient snsClient)
{
    //create a new SNS topic
    CreateTopicRequest createTopicRequest = new CreateTopicRequest("MyNewTopic200");
    CreateTopicResponse createTopicResponse = snsClient.CreateTopic(createTopicRequest);
    //get request id for CreateTopicRequest from SNS metadata		
    Console.WriteLine("CreateTopicRequest - " + createTopicResponse.ResponseMetadata.RequestId);
    return createTopicResponse.TopicArn;
}
```

When you run this example, the console output window of your IDE displays the following:

```
{TopicArn: arn:aws:sns:us-east-1:123456789012:mySNSTopic}
CreateTopicRequest - 93f7fc90-f131-5ca3-ab18-b741fef918b5
```

------

### Adding an SMS subscription to your topic<a name="sms_add_subscription_sdks"></a>

The following examples show how to add an SMS subscription to a topic using the Amazon SNS clients that the AWS SDKs provide\.

------
#### [ AWS SDK for Java ]

The following example uses the `subscribe` operation of the `AmazonSNSClient` class in the AWS SDK for Java:

```
public static void main(String[] args) {
        AmazonSNSClient snsClient = new AmazonSNSClient();
        String phoneNumber = "+1XXX5550100";
        String topicArn = createSNSTopic(snsClient);
        subscribeToTopic(snsClient, topicArn, "sms", phoneNumber);
}

//<create SNS topic>

public static void subscribeToTopic(AmazonSNSClient snsClient, String topicArn, 
		String protocol, String endpoint) {	
        SubscribeRequest subscribe = new SubscribeRequest(topicArn, protocol,
                                                          endpoint);
        SubscribeResult subscribeResult = snsClient.subscribe(subscribe);
        System.out.println("Subscribe request: " + 
                snsClient.getCachedResponseMetadata(subscribe));
        System.out.println("Subscribe result: " + subscribeResult);
}
```

This example constructs the `subscribeRequest` object and passes it the following arguments:
+ `topicArn` – The Amazon Resource Name \(ARN\) of the topic to which you are adding a subscription\.
+ `"sms"` – The protocol option for an SMS subscription\.
+ `endpoint` – The phone number that you are subscribing to the topic\.

The example uses the `getCachedResponseMetadata` operation to get the request ID for the subscribe request\.

When you run this example, the console window of your IDE displays the subscribe request ID:

```
SubscribeRequest - {AWS_REQUEST_ID=f38fe925-8093-5bd4-9c19-a7c7625de38c}
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `subscribe` operation of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET:

```
static void Main(string[] args)
{
    AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
    String phoneNumber = "+1XXX5550100";
    String topicArn = CreateSNSTopic(snsClient);
    SubscribeToTopic(snsClient, topicArn, "sms", phoneNumber);
}

//<create SNS topic>

static public void SubscribeToTopic(AmazonSimpleNotificationServiceClient snsClient, String topicArn,
    String protocol, String endpoint)
{
    SubscribeRequest subscribeRequest = new SubscribeRequest(topicArn, protocol, endpoint);
    SubscribeResponse subscribeResponse = snsClient.Subscribe(subscribeRequest);
    Console.WriteLine("Subscribe request: " + subscribeResponse.ResponseMetadata.RequestId);
    Console.WriteLine("Subscribe result: " + subscribeResponse.);
}
```

This example constructs the `SubscribeRequest` object and passes it the following arguments:
+ `topicArn` – The Amazon Resource Name \(ARN\) of the topic to which you are adding a subscription\.
+ `"sms"` – The protocol option for an SMS subscription\.
+ `endpoint` – The phone number that you are subscribing to the topic\.

When you run this example, the console window of your IDE displays the subscribe request ID:

```
SubscribeRequest - f38fe925-8093-5bd4-9c19-a7c7625de38c
```

------

### \(Optional\) Setting message attributes<a name="sms_set_attributes_sdks"></a>

The following examples show how to set message attributes using the Amazon SNS clients that the AWS SDKs provide\.

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

For more information about message attributes, see [Sending a message \(AWS SDKs\)](sms_publish-to-phone.md#sms_publish_sdk)\.

When you send an SMS message, you apply your attributes to the `PublishRequest` object\.

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET, you set message attribute values by adding attribute keys with `MessageAttributeValue` objects to the `MessageAttributes` field of the `PublishRequest` object\. Each `MessageAttributeValue` object is initialized with an attribute value, and each object declares the data type for the value\. The following example sets the sender ID to "mySenderID", maximum price to "0\.50" USD, and SMS type to "Promotional":

```
PublishRequest pubRequest = new PublishRequest();
pubRequest.MessageAttributes["AWS.SNS.SMS.SenderID"] = 
    new MessageAttributeValue{ StringValue = "mySenderId", DataType = "String"};
pubRequest.MessageAttributes["AWS.SNS.SMS.MaxPrice"] =
    new MessageAttributeValue { StringValue = "0.50", DataType = "Number" };
pubRequest.MessageAttributes["AWS.SNS.SMS.SMSType"] =
    new MessageAttributeValue { StringValue = "Promotional", DataType = "String" };
```

For more information about message attributes, see [Sending a message \(AWS SDKs\)](sms_publish-to-phone.md#sms_publish_sdk)\.

When you send an SMS message, you apply your attributes to the `PublishRequest` object\.

------

### Publishing a message to your topic<a name="sms_publish_to_topic_sdks"></a>

The following examples show how to publish a message to a topic using the Amazon SNS clients that the AWS SDKs provide\.

------
#### [ AWS SDK for Java ]

The following example uses the `publish` operation of the `AmazonSNSClient` class in the AWS SDK for Java:

```
public static void main(String[] args) {
        AmazonSNSClient snsClient = new AmazonSNSClient();
        String message = "My SMS message";
        Map<String, MessageAttributeValue> smsAttributes = 
                new HashMap<String, MessageAttributeValue>();
        //<set SMS attributes>
        String topicArn = createSNSTopic(snsClient);
        //<subscribe to topic>
        sendSMSMessageToTopic(snsClient, topicArn, message, smsAttributes);
}

//<create topic operation>

//<subscribe to topic operation>

public static void sendSMSMessageToTopic(AmazonSNSClient snsClient, String topicArn, 
		String message, Map<String, MessageAttributeValue> smsAttributes) {
        PublishResult result = snsClient.publish(new PublishRequest()
                        .withTopicArn(topicArn)
                        .withMessage(message)
                        .withMessageAttributes(smsAttributes));
        System.out.println(result);
}
```

Amazon SNS attempts to deliver that message to every phone number that is subscribed to the topic\.

This example constructs the `publishRequest` object while passing the topic ARN and the message as arguments\. The `publishResult` object captures the message ID that Amazon SNS returns\.

When you run this example, the console output window of your IDE displays the message ID:

```
{MessageId: 9b888f80-15f7-5c30-81a2-c4511a3f5229}
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `Publish` operation of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET:

```
public static void main(string[] args) 
{
        AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
        String topicArn = createSNSTopic(snsClient);
        PublishRequest pubRequest = new PublishRequest();
        pubRequest.Message = "My SMS message";
        pubRequest.TopicArn = topicArn;
        // add optional MessageAttributes...
        //   pubRequest.MessageAttributes["AWS.SNS.SMS.SenderID"] = 
        //      new MessageAttributeValue{ StringValue = "mySenderId", DataType = "String"};
        PublishResponse pubResponse = snsClient.Publish(pubRequest);
        Console.WriteLine(pubResponse.MessageId);
}

//<create topic operation>

//<subscribe to topic operation>
```

Amazon SNS attempts to deliver that message to every phone number that is subscribed to the topic\.

This example constructs the `publishRequest` object and assigns the topic ARN and the message\. The `publishResponse` object captures the message ID that Amazon SNS returns\.

When you run this example, the console output window of your IDE displays the message ID:

```
9b888f80-15f7-5c30-81a2-c4511a3f5229
```

------