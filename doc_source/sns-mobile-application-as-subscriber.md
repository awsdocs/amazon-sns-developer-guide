# Using Amazon SNS for User Notifications with a Mobile Application as a Subscriber \(Mobile Push\)<a name="sns-mobile-application-as-subscriber"></a>

With [Amazon SNS](https://aws.amazon.com/sns/), you have the ability to send push notification messages directly to apps on mobile devices\. Push notification messages sent to a mobile endpoint can appear in the mobile app as message alerts, badge updates, or even sound alerts\. 

**Important**  
Currently, you can create mobile applications only in the following Regions:  
US East \(N\. Virginia\)
US West \(N\. California\)
US West \(Oregon\)
Asia Pacific \(Mumbai\)
Asia Pacific \(Seoul\)
Asia Pacific \(Singapore\)
Asia Pacific \(Sydney\)
Asia Pacific \(Tokyo\)
EU \(Frankfurt\)
EU \(Ireland\)
South America \(São Paulo\)

## How User Notifications Work<a name="SNSMobilePushOverview"></a>

You send push notification messages to both mobile devices and desktops using one of the following supported push notification services: 
+ Amazon Device Messaging \(ADM\)
+ Apple Push Notification Service \(APNs\) for both iOS and Mac OS X
+ Baidu Cloud Push \(Baidu\)
+ Firebase Cloud Messaging \(FCM\)
+ Microsoft Push Notification Service for Windows Phone \(MPNS\)
+ Windows Push Notification Services \(WNS\)

Push notification services, such as APNs and FCM, maintain a connection with each app and associated mobile device registered to use their service\. When an app and mobile device register, the push notification service returns a device token\. Amazon SNS uses the device token to create a mobile endpoint, to which it can send direct push notification messages\. In order for Amazon SNS to communicate with the different push notification services, you submit your push notification service credentials to Amazon SNS to be used on your behalf\. For more information, see [Process Overview](#mobile-push-pseudo) 

 In addition to sending direct push notification messages, you can also use Amazon SNS to send messages to mobile endpoints subscribed to a topic\. The concept is the same as subscribing other endpoint types, such as Amazon SQS, HTTP/S, email, and SMS, to a topic, as described in [What is Amazon Simple Notification Service?](welcome.md)\. The difference is that Amazon SNS communicates using the push notification services in order for the subscribed mobile endpoints to receive push notification messages sent to the topic\. The following figure shows a mobile endpoint as a subscriber to an Amazon SNS topic\. The mobile endpoint communicates using push notification services where the other endpoints do not\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-subscribe.png)

## Prerequisites<a name="SNSMobilePushPrereq"></a>

To begin using Amazon SNS mobile push notifications, you need the following:
+ A set of credentials for connecting to one of the supported push notification services: ADM, APNs, Baidu, FCM, MPNS, or WNS\.
+ A device token or registration ID for the mobile app and device\.
+ Amazon SNS configured to send push notification messages to the mobile endpoints\.
+ A mobile app that is registered and configured to use one of the supported push notification services\.

 Registering your application with a push notification service requires several steps\. Amazon SNS needs some of the information you provide to the push notification service in order to send direct push notification messages to the mobile endpoint\. Generally speaking, you need the required credentials for connecting to the push notification service, a device token or registration ID \(representing your mobile device and mobile app\) received from the push notification service, and the mobile app registered with the push notification service\. 

The exact form the credentials take differs between mobile platforms, but in every case, these credentials must be submitted while making a connection to the platform\. One set of credentials is issued for each mobile app, and it must be used to send a message to any instance of that app\. 

The specific names will vary depending on which push notification service is being used\. For example, when using APNs as the push notification service, you need a *device token*\. Alternatively, when using FCM, the device token equivalent is called a *registration ID*\. The *device token* or *registration ID* is a string that is sent to the application by the operating system of the mobile device\. It uniquely identifies an instance of a mobile app running on a particular mobile device and can be thought of as unique identifiers of this app\-device pair\. 

Amazon SNS stores the credentials \(plus a few other settings\) as a platform application resource\. The device tokens \(again with some extra settings\) are represented as objects called platform endpoints\. Each platform endpoint belongs to one specific platform application, and every platform endpoint can be communicated with using the credentials that are stored in its corresponding platform application\.

The following sections include the prerequisites for each of the supported push notification services\. Once you've obtained the prerequisite information, you can send a push notification message using the AWS Management Console or the Amazon SNS mobile push APIs\. For more information, see [Process Overview](#mobile-push-pseudo)\. 

## Process Overview<a name="mobile-push-pseudo"></a>

To get started with Amazon SNS mobile push, you must first [obtain the credentials and device token](#SNSMobilePushPrereq) for the mobile platforms that you want to support\. Then, using the information from the mobile platforms, you can use Amazon SNS to [publish a message to a mobile device](mobile-push-send.md)\.

### Step 1: Request Credentials from Mobile Platforms<a name="mobile-push-creds"></a>

To use Amazon SNS mobile push, you must first request the necessary credentials from the mobile platforms\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-creds.png)

### Step 2: Request Token from Mobile Platforms<a name="mobile-push-token"></a>

 You then use the returned credentials to request a token for your mobile app and device from the mobile platforms\. The token you receive represents your mobile app and device\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-token.png)

### Step 3: Create Platform Application Object<a name="mobile-push-platform"></a>

The credentials and token are then used to create a platform application object \(`PlatformApplicationArn`\) from Amazon SNS\. For more information, see [Create a Platform Endpoint and Manage Device Tokens](mobile-platform-endpoint.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-platformapplicationobject.png)

### Step 4: Create Platform Endpoint Object<a name="mobile-push-endpoint"></a>

The `PlatformApplicationArn` is then used to create a platform endpoint object \(`EndpointArn`\) from Amazon SNS\. For more information, see [Create a Platform Endpoint and Manage Device Tokens](mobile-platform-endpoint.md)\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-endpointapplicationobject.png)

### Step 5: Publish Message to Mobile Endpoint<a name="mobile-push-publish"></a>

 The `EndpointArn` is then used to publish a message to an app on a mobile device\. For more information, see [Send a Direct Message to a Mobile Device](mobile-push-send-directmobile.md) and the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) API in the Amazon Simple Notification Service API Reference\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-mobile-publish.png)