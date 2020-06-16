# Text messaging \(SMS\)<a name="sns-mobile-phone-number-as-subscriber"></a>

You can use Amazon SNS to send text messages, or *SMS messages*, to SMS\-enabled devices\. You can [send a message directly to a phone number](sms_publish-to-phone.md), or you can [send a message to multiple phone numbers](sms_publish-to-topic.md) at once by subscribing those phone numbers to a topic and sending your message to the topic\.

You can [set SMS preferences](sms_preferences.md) for your AWS account to tailor your SMS deliveries for your use cases and budget\. For example, you can choose whether your messages are optimized for cost or reliable delivery\. You can also specify spending quotas for individual message deliveries and monthly spending quotas for your AWS account\.

Where required by local laws and regulations \(such as the US and Canada\), SMS recipients can [opt out](sms_manage.md#sms_manage_optout), which means that they choose to stop receiving SMS messages from your AWS account\. After a recipient opts out, you can, with limitations, opt in the phone number again so that you can resume sending messages to it\.

Amazon SNS supports SMS messaging in several regions, and you can send messages to more than 200 countries and regions\. For more information, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

**Topics**
+ [Originating identities for SMS messages](channels-sms-originating-identities.md)
+ [Requesting support for SMS messaging with Amazon SNS](channels-sms-awssupport.md)
+ [SMS best practices](channels-sms-best-practices.md)
+ [Setting SMS messaging preferences](sms_preferences.md)
+ [Sending an SMS message](sms_publish-to-phone.md)
+ [Sending an SMS message to multiple phone numbers](sms_publish-to-topic.md)
+ [Monitoring SMS activity](sms_stats.md)
+ [Managing phone numbers and SMS subscriptions](sms_manage.md)
+ [Supported Regions and countries](sns-supported-regions-countries.md)
+ [Special requirements for sending SMS messages to recipients in India](channels-sms-awssupport-sender-id-india.md)