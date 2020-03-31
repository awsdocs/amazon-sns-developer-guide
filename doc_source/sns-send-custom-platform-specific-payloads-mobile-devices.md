# Send custom platform\-specific payloads to mobile devices<a name="sns-send-custom-platform-specific-payloads-mobile-devices"></a>

You can use the AWS Management Console or Amazon SNS APIs to send custom messages with platform\-specific payloads to mobile devices\. For information about using the Amazon SNS APIs, see [Using Amazon SNS mobile push APIs](mobile-push-api.md) and the `SNSMobilePush.java` file in `[snsmobilepush\.zip](samples/snsmobilepush.zip)`\. 

## Sending JSON\-formatted messages<a name="mobile-push-send-json"></a>

When you send platform\-specific payloads, the data must be formatted as JSON key\-value pair strings, with the quotation marks escaped\.

The following examples show a custom message for the FCM platform\.

```
{
  "GCM":"{ \"notification\": { \"body\": \"Sample message for Android endpoints\", \"title\":\"TitleTest\" } }"
}
```

## Sending platform\-specific messages<a name="mobile-push-send-platform"></a>

In addition to sending custom data as key\-value pairs, you can send platform\-specific key\-value pairs\.

The following example shows the inclusion of the FCM parameters `time_to_live` and `collapse_key` after the custom data key\-value pairs in the FCM `data` parameter\.

```
{
   "GCM":"{  
       \"notification\": 
         { \"body\": \"Sample message for Android endpoints\", \"title\":\"TitleTest\" },
      \"data\":
         {\"time_to_live\": 3600,\"collapse_key\":\"deals\"}"
   }
}
```

For a list of the key\-value pairs supported by each of the push notification services supported in Amazon SNS, see the following: 
+ [Payload Key Reference](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/PayloadKeyReference.html#/apple_ref/doc/uid/TP40008194-CH17-SW1) in the APNs documentation
+ [Firebase Cloud Messaging HTTP Protocol](https://firebase.google.com/docs/cloud-messaging/http-server-ref) in the FCM documentation
+ [Send a Message](https://developer.amazon.com/sdk/adm/sending-message.html) in the ADM documentation

## Sending messages to an application on multiple platforms<a name="mobile-push-send-multiplatform"></a>

To send a message to an application installed on devices for multiple platforms, such as FCM and APNs, you must first subscribe the mobile endpoints to a topic in Amazon SNS and then publish the message to the topic\.

The following example shows a message to send to subscribed mobile endpoints on APNs, FCM, and ADM: 

```
{ 
  "default": "This is the default message which must be present when publishing a message to a topic. The default message will only be used if a message is not present for 
one of the notification platforms.",     
  "APNS": "{\"aps\":{\"alert\": \"Check out these awesome deals!\",\"url\":\"www.amazon.com\"} }",
  "GCM": "{\"data\":{\"message\":\"Check out these awesome deals!\",\"url\":\"www.amazon.com\"}}",
  "ADM": "{\"data\":{\"message\":\"Check out these awesome deals!\",\"url\":\"www.amazon.com\"}}" 
}
```

## Sending messages to APNs as alert or background notifications<a name="mobile-push-send-message-apns-background-notification"></a>

Amazon SNS can send messages to APNs as `alert` or `background` notifications \(for more information, see [Pushing Background Updates to Your App](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/pushing_background_updates_to_your_app) in the APNs documentation\)\.
+ An `alert` APNs notification informs the user by displaying an alert message, playing a sound, or adding a badge to your application’s icon\.
+ A `background` APNs notification wakes up or instructs your application to act upon the content of the notification, without informing the user\.

### Specifying custom APNs header values<a name="specify-custom-header-value"></a>

We recommend specifying custom values for the `AWS.SNS.MOBILE.APNS.PUSH_TYPE` [reserved message attribute](sns-message-attributes.md#sns-attrib-mobile-reserved) using the Amazon SNS `Publish` API action, AWS SDKs, or the AWS CLI\. The following CLI example sets `content-available` to `1` and `apns-push-type` to `background` for the specified topic\. 

```
aws sns publish \
--endpoint-url https://sns.us-east-1.amazonaws.com \
--target-arn arn:aws:sns:us-east-1:123456789012:endpoint/APNS_PLATFORM/MYAPP/1234a567-bc89-012d-3e45-6fg7h890123i \
--message '{"APNS_PLATFORM":"{\"aps\":{\"content-available\":1}}"}' \
--message-attributes '{ \
  "AWS.SNS.MOBILE.APNS.TOPIC":{"DataType":"String","StringValue":"com.amazon.mobile.messaging.myapp"}, \
  "AWS.SNS.MOBILE.APNS.PRIORITY":{"DataType":"String","StringValue":"10"}}', \
  "AWS.SNS.MOBILE.APNS.PUSH_TYPE":{"DataType":"String","StringValue":"background"} \
--message-structure json
```

### Inferring the APNs push type header from the payload<a name="inferring-push-type-header-from-payload"></a>

If you don't set the `apns-push-type` APNs header, Amazon SNS sets header to `alert` or `background` depending on the `content-available` key in the `aps` dictionary of your JSON\-formatted APNs payload configuration\.

**Note**  
Amazon SNS is able to infer only `alert` or `background` headers, although the `apns-push-type` header can be set to other values\.
+ `apns-push-type` is set to `alert`
  + If the `aps` dictionary contains `content-available` set to `1` and *one or more keys* that trigger user interactions\.
  + If the `aps` dictionary contains `content-available` set to `0` *or* if the `content-available` key is absent\.
  + If the value of the `content-available` key isn’t an integer or a Boolean\.
+ `apns-push-type` is set to `background`
  + If the `aps` dictionary *only* contains `content-available` set to `1` and *no other keys* that trigger user interactions\.
**Important**  
If Amazon SNS sends a raw configuration object for APNs as a background\-only notification, you must include `content-available` set to `1` in the `aps` dictionary\. Although you can include custom keys, the `aps` dictionary must not contain any keys that trigger user interactions \(for example, alerts, badges, or sounds\)\.

The following is an example raw configuration object\.

```
{
  "APNS": "{\"aps\":{\"content-available\":1},\"Foo1\":\"\Bar\",\"Foo2\":123}"
}
```

In this example, Amazon SNS sets the `apns-push-type` APNs header for the message to `background`\. When Amazon SNS detects that the `apn` dictionary contains the `content-available` key set to `1`—and doesn't contain any other keys that can trigger user interactions—it sets the header to `background`\.