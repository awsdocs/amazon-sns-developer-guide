# Reserving a dedicated short code for SMS messaging<a name="sqs-sms-short-codes"></a>

To send SMS messages using a persistent short code, you can reserve a dedicated short code that is assigned to your account and available exclusively to you\.

A short code is a 5 or 6 digit number that you can use to send SMS messages to certain destinations\. Short codes are often used for application\-to\-person \(A2P\) messaging, two\-factor authentication \(2FA\), and marketing\. Unless you reserve a short code, Amazon SNS will assign a short code to your messages\. This short code is shared with other Amazon SNS users, and it varies based upon destination and message type \(transactional or promotional\)\. By reserving a short code, you make it easier for your audience to recognize that your organization is the source of your messages\.

Your dedicated short code is available exclusively to you, so others are unable to message your audience using the same short code\. Consequently, your short code has some protection from malicious activity that might threaten your brand reputation or prompt wireless carriers to block your messages\.

Amazon SNS can use your short code to message telephone numbers in the United States\. For other destinations, Amazon SNS assigns a long code or alphanumeric code as required\.

A dedicated short code supports a higher delivery rate, which enables you send up to 100 SMS messages per second\. After you reserve a short code, you can request to increase this quota by [submitting a request](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\.

For pricing information, see [Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\.

**To reserve a dedicated short code**

1. Go to the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

1. Choose **Create case**\.

1. For **Regarding**, choose **Service Limit Increase**\.

1. For **Limit Type**, choose **SNS Text Messaging**\.

1. For **Resource Type**, choose **Dedicated SMS Short Codes for US destinations**\.

1. For **Limit**, choose the option that most closely resembles your use case\.

1. For **New limit value**, specify how many short codes you want to reserve \(typically, this value is `1`\)\.

1. For **Use Case Description**, summarize your use case, and summarize how your recipients will sign up for messages sent with your short code\.

1. Select your preferred language and contact method, and choose **Submit**\.

A customer service associate will contact you for additional information about your use case, including the customized messages your audience members will see when they reply to your short code with HELP or STOP\. AWS will work with wireless carriers on your behalf to provision your short code\. It typically takes 6 \- 12 weeks for all carriers to approve your use case and provision the short code so that you can send messages to the subscribers in their networks\. AWS will notify you when the short code provisioning is complete\.

To discontinue your short code reservation, send a request to AWS Support\.