# How user notifications work<a name="sns-how-user-notifications-work"></a>

You send push notification messages to both mobile devices and desktops using one of the following supported push notification services: 
+ Amazon Device Messaging \(ADM\)
+ Apple Push Notification Service \(APNs\) for both iOS and Mac OS X
+ Baidu Cloud Push \(Baidu\)
+ Firebase Cloud Messaging \(FCM\)
+ Microsoft Push Notification Service for Windows Phone \(MPNS\)
+ Windows Push Notification Services \(WNS\)

Push notification services, such as APNs and FCM, maintain a connection with each app and associated mobile device registered to use their service\. When an app and mobile device register, the push notification service returns a device token\. Amazon SNS uses the device token to create a mobile endpoint, to which it can send direct push notification messages\. In order for Amazon SNS to communicate with the different push notification services, you submit your push notification service credentials to Amazon SNS to be used on your behalf\. For more information, see [User notification process overview](sns-user-notifications-process-overview.md) 

 In addition to sending direct push notification messages, you can also use Amazon SNS to send messages to mobile endpoints subscribed to a topic\. The concept is the same as subscribing other endpoint types, such as Amazon SQS, HTTP/S, email, and SMS, to a topic, as described in [What is Amazon SNS?](welcome.md)\. The difference is that Amazon SNS communicates using the push notification services in order for the subscribed mobile endpoints to receive push notification messages sent to the topic\.