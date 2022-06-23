# Mobile push notifications best practices<a name="mobile-push-notifications-best-practices"></a>

This section describes best practices that might help you improve your customer engagement\.

## Endpoint management<a name="channels-sms-best-practices-endpoint-management"></a>

Delivery issues might occur in situations were device tokens change due to a user’s action on the device \(e\.g\. an app is re\-installed on the device\), or [certificate updates](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns) affecting devices running on a particular iOS version\. It is a recommended best practice by Apple to [register](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#:~:text=Registering%20to%20Receive%20Remote%20Notifications) with APNs each time your app launches\.

Since the device token won’t change each time an app is opened by a user, the idempotent [https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformEndpoint.html](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformEndpoint.html) API can be used\. However, this can introduce duplicates for the same device in cases where the token itself is invalid, or if the endpoint is valid but disabled \(e\.g\. a mismatch of production and sandbox environments\)\.

A device token management mechanism such as the one in the [pseudo code](mobile-platform-endpoint.md#mobile-platform-endpoint-pseudo-code) can be used\.

## Delivery status logging<a name="channels-sms-best-practices-delivery-logging"></a>

To monitor push notification delivery status, we recommended you enable delivery status logging for your Amazon SNS platform application\. This helps you troubleshoot delivery failures because the logs contain provider [response codes](sns-msg-status.md#platform-returncodes) returned from the push platform service\. For details on enabling delivery status logging, see [How do I access Amazon SNS topic delivery logs for push notifications?](https://aws.amazon.com/premiumsupport/knowledge-center/troubleshoot-failed-sns-deliveries/)\.

## Event notifications<a name="channels-sms-best-practices-event-notifications"></a>

For managing endpoints in an event driven fashion, you can make use of the [event notifications](application-event-notifications.md#application-event-notifications-sdk) functionality\. This allows the configured Amazon SNS topic to fanout events to the subscribers such as a Lambda function, for platform application events of endpoint creation, deletion, updates, and delivery failures\.