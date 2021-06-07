# Deleting phone numbers from the SMS sandbox<a name="sns-sms-sandbox-deleting-phone-numbers"></a>

You can delete pending or verified destination phone numbers from the [SMS sandbox](sns-sms-sandbox.md)\.

**To delete destination phone numbers from the SMS sandbox**

1. Wait 24 hours after [verifying the phone number](sns-sms-sandbox-verifying-phone-numbers.md), or 24 hours after your last verification attempt\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, choose an [AWS Region that supports SMS messaging](sns-supported-regions-countries.md) where you added a destination phone number\.

1. In the navigation pane, choose **Text messaging \(SMS\)**\.

1. On the **Mobile text messaging \(SMS\)** page, under **Sandbox destination phone numbers**, choose the phone number to delete, and then choose **Delete phone number**\.

1. To confirm that you want to delete the phone number, enter **delete me**, and then choose **Delete**\.

   If 24 hours or more have passed since you verified or attempted to verify the destination phone number, it is deleted, and Amazon SNS updates the list of your destination phone numbers\.

1. Repeat these steps in each Region where you added the destination phone number and no longer plan to use it\.