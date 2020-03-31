# Using the Amazon SNS time to live \(TTL\) message attribute for mobile push notifications<a name="sns-ttl"></a>

Amazon Simple Notification Service \(Amazon SNS\) provides support for setting a *Time To Live \(TTL\)* message attribute for mobile push notifications messages\. This is in addition to the existing capability of setting TTL within the Amazon SNS message body for the mobile push notification services that support this, such as Amazon Device Messaging \(ADM\) and Firebase Cloud Messaging \(FCM\)\.

The TTL message attribute is used to specify expiration metadata about a message\. This allows you to specify the amount of time that the push notification service, such as Apple Push Notification Service \(APNs\) or FCM, has to deliver the message to the endpoint\. If for some reason \(such as the mobile device has been turned off\) the message is not deliverable within the specified TTL, then the message will be dropped and no further attempts to deliver it will be made\. To specify TTL within message attributes, you can use the AWS Management Console, AWS software development kits \(SDKs\), or query API\. 

**Topics**
+ [TTL message attributes for push notification services](#sns-ttl-msg-attrib)
+ [Precedence order for determining TTL](#sns-ttl-precedence)
+ [Specifying TTL using the AWS Management Console](#sns-ttl-console)
+ [Specifying TTL with the AWS SDKs](#sns-ttl-sdk)

## TTL message attributes for push notification services<a name="sns-ttl-msg-attrib"></a>

The following is a list of the TTL message attributes for push notification services that you can use to set when using the AWS SDKs or query API:


****  

| Push notification service | TTL message attribute | 
| --- | --- | 
| Amazon Device Messaging \(ADM\) | AWS\.SNS\.MOBILE\.ADM\.TTL | 
| Apple Push Notification Service \(APNs\) | AWS\.SNS\.MOBILE\.APNS\.TTL | 
| Apple Push Notification Service Sandbox \(APNs\_SANDBOX\) | AWS\.SNS\.MOBILE\.APNS\_SANDBOX\.TTL | 
| Baidu Cloud Push \(Baidu\) | AWS\.SNS\.MOBILE\.BAIDU\.TTL | 
| Firebase Cloud Messaging \(FCM\) | AWS\.SNS\.MOBILE\.FCM\.TTL | 
| Windows Push Notification Services \(WNS\) | AWS\.SNS\.MOBILE\.WNS\.TTL | 

Each of the push notification services handle TTL differently\. Amazon SNS provides an abstract view of TTL over all the push notification services, which makes it easier to specify TTL\. When you use the AWS Management Console to specify TTL \(in seconds\), you only have to enter the TTL value once and Amazon SNS will then calculate the TTL for each of the selected push notification services when publishing the message\. 

 TTL is relative to the publish time\. Before handing off a push notification message to a specific push notification service, Amazon SNS computes the dwell time \(the time between the publish timestamp and just before handing off to a push notification service\) for the push notification and passes the remaining TTL to the specific push notification service\. If TTL is shorter than the dwell time, Amazon SNS won't attempt to publish\. 

If you specify a TTL for a push notification message, then the TTL value must be a positive integer, unless the value of `0` has a specific meaning for the push notification service—such as with APNs and FCM\. If the TTL value is set to `0` and the push notification service does not have a specific meaning for `0`, then Amazon SNS will drop the message\. For more information about the TTL parameter set to `0` when using APNs, see *Table A\-3 Item identifiers for remote notifications* in the [Binary Provider API](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/BinaryProviderAPI.html) documentation\.

## Precedence order for determining TTL<a name="sns-ttl-precedence"></a>

The precedence that Amazon SNS uses to determine the TTL for a push notification message is based on the following order, where the lowest number has the highest priority: 

1. Message attribute TTL

1. Message body TTL

1. Push notification service default TTL \(varies per service\)

1. Amazon SNS default TTL \(4 weeks\)

If you set different TTL values \(one in message attributes and another in the message body\) for the same message, then Amazon SNS will modify the TTL in the message body to match the TTL specified in the message attribute\.

## Specifying TTL using the AWS Management Console<a name="sns-ttl-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Mobile**, **Push notifications**\.

1. On the **Mobile push notifications** page, in the **Platform applications** section, choose an application\.

1. On the ***MyApplication*** page, in the **Endpoints** section, choose an application endpoint and then choose **Publish message**\.

1. In the **Message details** section, enter the TTL \(the number of seconds that the push notification service has to deliver the message to the endpoint\)\.

1. Choose **Publish message**\.

## Specifying TTL with the AWS SDKs<a name="sns-ttl-sdk"></a>

The [AWS SDKs](http://aws.amazon.com/tools/) provide APIs in several languages for using TTL with Amazon SNS\. 

For more information about the SDK for Java, see [Getting Started with the AWS SDK for Java](https://aws.amazon.com/developers/getting-started/java/)\.

The following Java example shows how to configure a TTL message attribute and publish the message to an endpoint, which in this example is registered with Baidu Cloud Push:

```
Map<String, MessageAttributeValue> messageAttributes = new HashMap<String, MessageAttributeValue>();

// Insert your desired value (in seconds) of TTL here. For example, a TTL of 1 day would be 86,400 seconds. 
messageAttributes.put("AWS.SNS.MOBILE.BAIDU.TTL", new MessageAttributeValue().withDataType("String").withStringValue("86400"));

PublishRequest publishRequest = new PublishRequest();
publishRequest.setMessageAttributes(messageAttributes);
String message = "{\"title\":\"Test_Title\",\"description\":\"Test_Description\"}";
publishRequest.setMessage(message);
publishRequest.setMessageStructure("json");
publishRequest.setTargetArn("arn:aws:sns:us-east-2:999999999999:endpoint/BAIDU/TestApp/318fc7b3-bc53-3d63-ac42-e359468ac730");
PublishResult publishResult = snsClient.publish(publishRequest);
```

For more information about using message attributes with Amazon SNS, see [Amazon SNS message attributes](sns-message-attributes.md)\. 