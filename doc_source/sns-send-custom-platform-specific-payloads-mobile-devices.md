# Send Custom Platform\-Specific Payloads to Mobile Devices<a name="sns-send-custom-platform-specific-payloads-mobile-devices"></a>

You can use the AWS Management Console or Amazon SNS APIs to send custom messages with platform\-specific payloads to mobile devices\. For information about using the Amazon SNS APIs, see [Using Amazon SNS Mobile Push APIs](mobile-push-api.md) and the `SNSMobilePush.java` file in `[snsmobilepush\.zip](samples/snsmobilepush.zip)`\. 

## Sending JSON\-Formatted Messages<a name="mobile-push-send-json"></a>

When you send platform\-specific payloads, the data must be formatted as JSON key\-value pair strings, with the quotation marks escaped\.

The following examples show a custom message for the FCM platform\.

```
{
  "FCM":"{\"data\":{\"message\":\"Check out these awesome deals!\",\"url\":\"www.amazon.com\"}}"
}
```

## Sending Platform\-Specific Messages<a name="mobile-push-send-platform"></a>

In addition to sending custom data as key\-value pairs, you can send platform\-specific key\-value pairs\.

The following example shows the inclusion of the FCM parameters `time_to_live` and `collapse_key` after the custom data key\-value pairs in the FCM `data` parameter\.

```
{
  "FCM":"{\"data\":{\"message\":\"Check out these awesome deals!\",\"url\":\"www.amazon.com\"},\"time_to_live\": 3600,\"collapse_key\":\"deals\"}"
}
```

For a list of the key\-value pairs supported by each of the push notification services supported in Amazon SNS, see the following: 
+ [Payload Key Reference](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#//apple_ref/doc/uid/TP40008194-CH17-SW1) in the APNs documentation
+ [Firebase Cloud Messaging HTTP Protocol](https://firebase.google.com/docs/cloud-messaging/http-server-ref) in the FCM documentation
+ [Send a Message](https://developer.amazon.com/sdk/adm/sending-message.html) in the ADM documentation

## Sending Messages to an Application on Multiple Platforms<a name="mobile-push-send-multiplatform"></a>

To send a message to an application installed on devices for multiple platforms, such as FCM and APNs, you must first subscribe the mobile endpoints to a topic in Amazon SNS and then publish the message to the topic\.

The following example shows a message to send to subscribed mobile endpoints on APNs, FCM, and ADM: 

```
{ 
  "default": "This is the default message which must be present when publishing a message to a topic. The default message will only be used if a message is not present for 
one of the notification platforms.",     
  "APNS": "{\"aps\":{\"alert\": \"Check out these awesome deals!\",\"url\":\"www.amazon.com\"} }",
  "FCM": "{\"data\":{\"message\":\"Check out these awesome deals!\",\"url\":\"www.amazon.com\"}}",
  "ADM": "{ \"data\": { \"message\": \"Check out these awesome deals!\",\"url\":\"www.amazon.com\" }}" 
}
```