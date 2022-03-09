# Moving out of the SMS sandbox<a name="sns-sms-sandbox-moving-to-production"></a>

Moving your AWS account out of the [SMS sandbox](sns-sms-sandbox.md) requires that you first add, verify, and test destination phone numbers\. Then, you must create a case with AWS Support\.

**To request that your AWS account is moved out of the SMS sandbox**

1. While your AWS account is in the SMS sandbox, [add and verify](sns-sms-sandbox-verifying-phone-numbers.md) one or more destination phone numbers\.

1. Confirm that you can publish and receive messages to at least one verified destination phone number\.

   For instructions on publishing SMS messages to one or more phone numbers, see [Publishing to a mobile phone](sms_publish-to-phone.md)\.

1. On the Amazon SNS console's **Mobile text messaging \(SMS\)** page, under **Account information**, choose **Exit SMS sandbox**\.

   The [AWS Support Center](https://console.aws.amazon.com/support/home#/) opens and creates a case\. The **Service limit increase** option is already selected and the **Limit type** is set to **SNS Text Messaging**\.

1. Under **Case details**, provide the following information:
   + A link to the site or the name of the app that you plan to send SMS messages from
   + The type of messages that you plan to send: one\-time passwords, promotional, or transactional
   + The AWS Region that you plan to send SMS messages from
   + The countries or regions that you plan to send SMS messages to
   + A description of how your customers opt in to receive messages from you
   + Any message template that you plan to use

1. Under **Requests**, do the following:

   1. For **Region**, choose the AWS Region where you want to move your AWS account out of the SMS sandbox\.

   1. For **Resource Type**, choose **General Limits**\.

   1. For **Limit**, choose **Exit SMS Sandbox**\.

   1. \(Optional\) To make additional requests, such as a limit increase, choose **Add another request**\. Then, choose the options for your request\.

1. \(Optional\) Under **Case description**, enter any additional relevant details about your request\.

1. Under **Contact options**, choose your **Preferred contact language**\.

1. Choose **Submit**\.

The AWS Support team provides an initial response to your request within 24 hours\.

To prevent our systems from being used to send unsolicited or malicious content, we consider each request carefully\. If we can, we will grant your request within this 24\-hour period\. However, if we need additional information from you, it might take longer to resolve your request\.

If your use case doesn't align with our policies, we might be unable to grant your request\.