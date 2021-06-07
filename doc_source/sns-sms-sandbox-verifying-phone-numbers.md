# Adding and verifying phone numbers in the SMS sandbox<a name="sns-sms-sandbox-verifying-phone-numbers"></a>

To get started with sending SMS messages while your AWS account is in the [SMS sandbox](sns-sms-sandbox.md), first add destination phone numbers, and then verify them\.

**Note**  
As with accounts that aren't in the SMS sandbox, an [origination identity](channels-sms-originating-identities.md) is required before you can send SMS messages to recipients in some countries or regions\. For more information, see [Supported Regions and countries](sns-supported-regions-countries.md)\.  
Origination IDs include [sender IDs](channels-sms-originating-identities-sender-ids.md) and different types of [origination numbers](channels-sms-originating-identities-origination-numbers.md)\. To view your existing origination numbers, in the navigation pane of the [Amazon SNS console](https://console.aws.amazon.com/sns/home), choose **Origination numbers**\. Currently, sender IDs don't appear in this list\.

**To add and verify destination phone numbers**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, choose an [AWS Region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. In the navigation pane, choose **Text messaging \(SMS\)**\.

1. On the **Mobile text messaging \(SMS\)** page, under **Sandbox destination phone numbers**, choose **Add phone number**\.

1. Under **Destination details**, enter the country code and phone number, specify what language to use for the verification message, and then choose **Add phone number**\.

   Amazon SNS sends a one\-time password \(OTP\) to the destination phone number\. If the destination phone number doesn't receive the OTP within 15 minutes, choose **Resend verification code**\. You can send the OTP to the same destination phone number up to five times every 24 hours\.

1. In the **Verification code** box, enter the OTP sent to the destination phone number, and then choose **Verify phone number**\.

   The destination phone number and its verification status appear in the **Sandbox destination phone numbers** section\. If the verification status is **Pending**, verification was unsuccessful\. This can happen, for example, if you didn't enter the country code with the phone number\. You can delete pending or verified destination phone numbers only after 24 hours or more have passed since verification or the last verification attempt\.

1. Repeat these steps in each Region where you want to use this destination phone number\.