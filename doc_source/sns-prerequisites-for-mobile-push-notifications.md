# Prerequisites for Amazon SNS user notifications<a name="sns-prerequisites-for-mobile-push-notifications"></a>

To begin using Amazon SNS mobile push notifications, you need the following:
+ A set of credentials for connecting to one of the supported push notification services: ADM, APNs, Baidu, FCM, MPNS, or WNS\.
+ A device token or registration ID for the mobile app and device\.
+ Amazon SNS configured to send push notification messages to the mobile endpoints\.
+ A mobile app that is registered and configured to use one of the supported push notification services\.

 Registering your application with a push notification service requires several steps\. Amazon SNS needs some of the information you provide to the push notification service in order to send direct push notification messages to the mobile endpoint\. Generally speaking, you need the required credentials for connecting to the push notification service, a device token or registration ID \(representing your mobile device and mobile app\) received from the push notification service, and the mobile app registered with the push notification service\. 

The exact form the credentials take differs between mobile platforms, but in every case, these credentials must be submitted while making a connection to the platform\. One set of credentials is issued for each mobile app, and it must be used to send a message to any instance of that app\. 

The specific names will vary depending on which push notification service is being used\. For example, when using APNs as the push notification service, you need a *device token*\. Alternatively, when using FCM, the device token equivalent is called a *registration ID*\. The *device token* or *registration ID* is a string that is sent to the application by the operating system of the mobile device\. It uniquely identifies an instance of a mobile app running on a particular mobile device and can be thought of as unique identifiers of this app\-device pair\. 

Amazon SNS stores the credentials \(plus a few other settings\) as a platform application resource\. The device tokens \(again with some extra settings\) are represented as objects called platform endpoints\. Each platform endpoint belongs to one specific platform application, and every platform endpoint can be communicated with using the credentials that are stored in its corresponding platform application\.

The following sections include the prerequisites for each of the supported push notification services\. Once you've obtained the prerequisite information, you can send a push notification message using the AWS Management Console or the Amazon SNS mobile push APIs\. For more information, see [User notification process overview](sns-user-notifications-process-overview.md)\. 