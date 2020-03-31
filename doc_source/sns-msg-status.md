# Using Amazon SNS application attributes for message delivery status<a name="sns-msg-status"></a>

Amazon Simple Notification Service \(Amazon SNS\) provides support to log the delivery status of push notification messages\. After you configure application attributes, log entries will be sent to CloudWatch Logs for messages sent from Amazon SNS to mobile endpoints\. Logging message delivery status helps provide better operational insight, such as the following: 
+ Know whether a push notification message was delivered from Amazon SNS to the push notification service\.
+ Identify the response sent from the push notification service to Amazon SNS\.
+ Determine the message dwell time \(the time between the publish timestamp and just before handing off to a push notification service\)\.

 To configure application attributes for message delivery status, you can use the AWS Management Console, AWS software development kits \(SDKs\), or query API\. 

**Topics**
+ [Configuring message delivery status attributes using the AWS Management Console](#sns-msg-console)
+ [Amazon SNS message delivery status CloudWatch log examples](#sns-msg-examples)
+ [Configuring message delivery status attributes with the AWS SDKs](#sns-msg-sdk)
+ [Platform response codes](#platform-returncodes)

## Configuring message delivery status attributes using the AWS Management Console<a name="sns-msg-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Apps**, and then choose the app containing the endpoints for which you want receive CloudWatch Logs\.

1. Choose **Application Actions** and then choose **Delivery Status**\.

1. On the **Delivery Status** dialog box, choose **Create IAM Roles**\.

   You will then be redirected to the IAM console\.

1. Choose **Allow** to give Amazon SNS write access to use CloudWatch Logs on your behalf\.

1. Now, back on the **Delivery Status** dialog box, enter a number in the **Percentage of Success to Sample \(0\-100\)** field for the percentage of successful messages sent for which you want to receive CloudWatch Logs\.
**Note**  
After you configure application attributes for message delivery status, all failed message deliveries generate CloudWatch Logs\.

1. Finally, choose **Save Configuration**\. You will now be able to view and parse the CloudWatch Logs containing the message delivery status\. For more information about using CloudWatch, see the [CloudWatch Documentation](https://aws.amazon.com/documentation/cloudwatch)\.

## Amazon SNS message delivery status CloudWatch log examples<a name="sns-msg-examples"></a>

After you configure message delivery status attributes for an application endpoint, CloudWatch Logs will be generated\. Example logs, in JSON format, are shown as follows:

**SUCCESS**

```
{
  "status": "SUCCESS",
  "notification": {
    "timestamp": "2015-01-26 23:07:39.54",
    "messageId": "9655abe4-6ed6-5734-89f7-e6a6a42de02a"
  },
  "delivery": {
    "statusCode": 200,
    "dwellTimeMs": 65,
    "token": "Examplei7fFachkJ1xjlqT64RaBkcGHochmf1VQAr9k-IBJtKjp7fedYPzEwT_Pq3Tu0lroqro1cwWJUvgkcPPYcaXCpPWmG3Bqn-wiqIEzp5zZ7y_jsM0PKPxKhddCzx6paEsyay9Zn3D4wNUJb8m6HXrBf9dqaEw",
    "attempts": 1,
    "providerResponse": "{\"multicast_id\":5138139752481671853,\"success\":1,\"failure\":0,\"canonical_ids\":0,\"results\":[{\"message_id\":\"0:1422313659698010%d6ba8edff9fd7ecd\"}]}",
    "destination": "arn:aws:sns:us-east-2:111122223333:endpoint/FCM/FCMPushApp/c23e42de-3699-3639-84dd-65f84474629d"
  }
}
```

**FAILURE**

```
{
  "status": "FAILURE",
  "notification": {
    "timestamp": "2015-01-26 23:29:35.678",
    "messageId": "c3ad79b0-8996-550a-8bfa-24f05989898f"
  },
  "delivery": {
    "statusCode": 8,
    "dwellTimeMs": 1451,
    "token": "examp1e29z6j5c4df46f80189c4c83fjcgf7f6257e98542d2jt3395kj73",
    "attempts": 1,
    "providerResponse": "NotificationErrorResponse(command=8, status=InvalidToken, id=1, cause=null)",
    "destination": "arn:aws:sns:us-east-2:111122223333:endpoint/APNS_SANDBOX/APNSPushApp/986cb8a1-4f6b-34b1-9a1b-d9e9cb553944"
  }
}
```

For a list of push notification service response codes, see [Platform response codes](#platform-returncodes)\.

## Configuring message delivery status attributes with the AWS SDKs<a name="sns-msg-sdk"></a>

The [AWS SDKs](https://aws.amazon.com/tools/) provide APIs in several languages for using message delivery status attributes with Amazon SNS\. 

The following Java example shows how to use the `SetPlatformApplicationAttributes` API to configure application attributes for message delivery status of push notification messages\. You can use the following attributes for message delivery status: `SuccessFeedbackRoleArn`, `FailureFeedbackRoleArn`, and `SuccessFeedbackSampleRate`\. The `SuccessFeedbackRoleArn` and `FailureFeedbackRoleArn` attributes are used to give Amazon SNS write access to use CloudWatch Logs on your behalf\. The `SuccessFeedbackSampleRate` attribute is for specifying the sample rate percentage \(0\-100\) of successfully delivered messages\. After you configure the `FailureFeedbackRoleArn` attribute, then all failed message deliveries generate CloudWatch Logs\. 

```
SetPlatformApplicationAttributesRequest setPlatformApplicationAttributesRequest = new SetPlatformApplicationAttributesRequest();
Map<String, String> attributes = new HashMap<>();
attributes.put("SuccessFeedbackRoleArn", "arn:aws:iam::111122223333:role/SNS_CWlogs");
attributes.put("FailureFeedbackRoleArn", "arn:aws:iam::111122223333:role/SNS_CWlogs");
attributes.put("SuccessFeedbackSampleRate", "5");
setPlatformApplicationAttributesRequest.withAttributes(attributes);
setPlatformApplicationAttributesRequest.setPlatformApplicationArn("arn:aws:sns:us-west-2:111122223333:app/FCM/FCMPushApp");
sns.setPlatformApplicationAttributes(setPlatformApplicationAttributesRequest);
```

For more information about the SDK for Java, see [Getting Started with the AWS SDK for Java](https://aws.amazon.com/developers/getting-started/java/)\.

## Platform response codes<a name="platform-returncodes"></a>

The following is a list of links for the push notification service response codes:


****  

| Push notification service | Response codes | 
| --- | --- | 
| Amazon Device Messaging \(ADM\) | See [Response Format](https://developer.amazon.com/docs/adm/send-message.html#response-format) in the ADM documentation\. | 
| Apple Push Notification Service \(APNs\) | See HTTP/2 Response from APNs in [Communicating with APNs](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CommunicatingwithAPNs.html#/apple_ref/doc/uid/TP40008194-CH11-SW1) in the Local and Remote Notification Programming Guide\. | 
| Firebase Cloud Messaging \(FCM\) | See [Downstream Message Error Response Codes](https://firebase.google.com/docs/cloud-messaging/http-server-ref#error-codes) in the Firebase Cloud Messaging documentation\. | 
| Microsoft Push Notification Service for Windows Phone \(MPNS\) | See [Push Notification Service Response Codes for Windows Phone 8](https://msdn.microsoft.com/en-us/library/windows/apps/ff941100%28v=vs.105%29.aspx#BKMK_PushNotificationServiceResponseCodes) in the Windows 8 Development documentation\. | 
| Windows Push Notification Services \(WNS\) | See "Response codes" in [Push Notification Service Request and Response Headers \(Windows Runtime Apps\)](https://msdn.microsoft.com/en-us/library/windows/apps/hh465435.aspx) in the Windows 8 Development documentation\. | 