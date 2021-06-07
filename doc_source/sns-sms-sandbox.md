# SMS sandbox<a name="sns-sms-sandbox"></a>

When you start using Amazon SNS to send SMS messages, your AWS account is in the *SMS sandbox*\. The SMS sandbox provides a safe environment for you to try Amazon SNS features without risking your reputation as an SMS sender\. While your account is in the SMS sandbox, you can use all of the features of Amazon SNS, with the following restrictions:
+ You can send SMS messages only to verified destination phone numbers\.
+ You can have up to 10 verified destination phone numbers\.
+ You can delete destination phone numbers only after 24 or more hours have passed since verification or the last verification attempt\.

When your account is moved out of the sandbox, these restrictions are removed, and you can send SMS messages to any recipient\.

**Topics**
+ [Adding and verifying phone numbers in the SMS sandbox](sns-sms-sandbox-verifying-phone-numbers.md)
+ [Deleting phone numbers from the SMS sandbox](sns-sms-sandbox-deleting-phone-numbers.md)
+ [Moving out of the SMS sandbox](sns-sms-sandbox-moving-to-production.md)