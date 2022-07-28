# Mobile push notifications<a name="sns-mobile-application-as-subscriber"></a>

With [Amazon SNS](https://aws.amazon.com/sns/), you have the ability to send push notification messages directly to apps on mobile devices\. Push notification messages sent to a mobile endpoint can appear in the mobile app as message alerts, badge updates, or even sound alerts\. 

**Topics**
+ [How user notifications work](#sns-how-user-notifications-work)
+ [User notification process overview](#sns-user-notifications-process-overview)
+ [Setting up a mobile app](mobile-push-send.md)
+ [Sending mobile push notifications](mobile-push-notifications.md)
+ [Mobile app attributes](sns-msg-status.md)
+ [Mobile app events](application-event-notifications.md)
+ [Mobile push API actions](mobile-push-api.md)
+ [Mobile push API errors](mobile-push-api-error.md)
+ [Using the Amazon SNS time to live \(TTL\) message attribute for mobile push notifications](sns-ttl.md)
+ [Supported Regions for mobile applications](sns-mobile-push-supported-regions.md)
+ [Mobile push notifications best practices](mobile-push-notifications-best-practices.md)

## How user notifications work<a name="sns-how-user-notifications-work"></a>

You send push notification messages to both mobile devices and desktops using one of the following supported push notification services: 
+ Amazon Device Messaging \(ADM\)
+ Apple Push Notification Service \(APNs\) for both iOS and Mac OS X
+ Baidu Cloud Push \(Baidu\)
+ Firebase Cloud Messaging \(FCM\)
+ Microsoft Push Notification Service for Windows Phone \(MPNS\)
+ Windows Push Notification Services \(WNS\)

Push notification services, such as APNs and FCM, maintain a connection with each app and associated mobile device registered to use their service\. When an app and mobile device register, the push notification service returns a device token\. Amazon SNS uses the device token to create a mobile endpoint, to which it can send direct push notification messages\. In order for Amazon SNS to communicate with the different push notification services, you submit your push notification service credentials to Amazon SNS to be used on your behalf\. For more information, see [User notification process overview](#sns-user-notifications-process-overview)\. 

 In addition to sending direct push notification messages, you can also use Amazon SNS to send messages to mobile endpoints subscribed to a topic\. The concept is the same as subscribing other endpoint types, such as Amazon SQS, HTTP/S, email, and SMS, to a topic, as described in [What is Amazon SNS?](welcome.md)\. The difference is that Amazon SNS communicates using the push notification services in order for the subscribed mobile endpoints to receive push notification messages sent to the topic\.

## User notification process overview<a name="sns-user-notifications-process-overview"></a>

1. [Obtain the credentials and device token](sns-prerequisites-for-mobile-push-notifications.md) for the mobile platforms that you want to support\.

1. Use the credentials to create a platform application object \(`PlatformApplicationArn`\) using Amazon SNS\. For more information, see [Creating a platform application](mobile-push-send-register.md)\.

1. Use the returned credentials to request a device token for your mobile app and device from the push notification service\. The token you receive represents your mobile app and device\.

1. Use the device token and the `PlatformApplicationArn` to create a platform endpoint object \(`EndpointArn`\) using Amazon SNS\. For more information, see [Creating a platform endpoint](mobile-platform-endpoint.md)\.

1. Use the `EndpointArn` to [publish a message to an app on a mobile device](mobile-push-send.md)\. For more information, see [Publishing to a mobile device](mobile-push-send-directmobile.md) and the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) API in the Amazon Simple Notification Service API Reference\.
