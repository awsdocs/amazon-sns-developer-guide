# Publish SMS messages to an Amazon SNS topic using an AWS SDK<a name="example_sns_UsageSmsTopic_section"></a>

The following code example shows how to:
+ Create an Amazon SNS topic\.
+ Subscribe phone numbers to the topic\.
+ Publish SMS messages to the topic so that all subscribed phone numbers receive the message at once\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 1\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/java/example_code/sns#code-examples)\. 
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

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SNS with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.