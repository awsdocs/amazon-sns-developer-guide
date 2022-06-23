# Requesting increases to your monthly SMS spending quota for Amazon SNS<a name="channels-sms-awssupport-spend-threshold"></a>

Amazon SNS provides spending quotas to help you manage the maximum per\-month cost incurred by sending SMS using your account\. The spending quota limits your risk in case of malicious attack, and prevents your upstream application from sending more messages than expected\. You can configure Amazon SNS to stop publishing SMS messages when it determines that sending an SMS message will incur a cost that exceeds your spending quota for the current month\. 

To ensure your operations are not impacted, we recommend requesting a spending quota high enough to support your production workloads\. For more information, see [Step 1: Open an Amazon SNS SMS case](#channels-sms-awssupport-spend-threshold-open)\. Once you have received the quota, you can manage your risk by applying the full quota, or a smaller value, as described in [Step 2: Update your SMS settings](#channels-sms-awssupport-spend-threshold-settings)\. By applying a smaller value, you can control your monthly spending with the option to scale up if necessary\.

**Important**  
Because Amazon SNS is a distributed system, it stops sending SMS messages within minutes if the spending quota is exceeded\. During this period, if you continue to send SMS messages, you might incur costs that exceed your quota\.

We set the spending quota for all new accounts at $1\.00 \(USD\) per month\. This quota is intended to let you test the message\-sending capabilities of Amazon SNS\. To request an increase to the SMS spending quota for your account, open a quota increase case in the AWS Support Center\.

## Step 1: Open an Amazon SNS SMS case<a name="channels-sms-awssupport-spend-threshold-open"></a>

You can request an increase to your monthly spending quota by opening a quota increase case in the AWS Support Center\.

**Note**  
Some of the fields on the request form are marked as "optional\." However, AWS Support requires all of the information that's mentioned in the following steps in order to process your request\. If you don't provide all of the required information, you may experience delays in processing your request\.

**To request a spending quota increase**

1. Sign in to the [AWS Support Center](https://us-east-1.console.aws.amazon.com/support/home?region=us-east-1&skipRegion=true#/case/create?issueType=service-limit-increase)\. 

1. Under **Create case**, choose **Service limit increase**, and then under **Case details**, perform the following steps:

1. For **Limit type**, choose **SNS Text Messaging**\.

1. For **Provide a link to the site or app which will be sending SMS messages \- optional**, enter the URL of your website or application\.

1. For **What type of messages do you plan to send? \- optional**, choose **One Time Password**, **Promotional**, or **Transactional**, depending on what you plan to send\.

1. For **Targeted Countries** \- optional, choose **General Limits**\.

1. Under **Requests**, for Request 1, do the following:

1. For **Resource Type**, choose **General Limits**\.

1. For **New limit**, enter the needed spend limit that you calculated earlier\.

1. Under **Case description**, for **Use case description**, enter the description that you wrote earlier\.

1. Expand **Contact options**, and then choose your preferred contact language\.

1. Choose **Submit**\.

1. When you finish, choose **Submit**\. 

The AWS Support team provides an initial response to your request within 24 hours\.

To prevent our systems from being used to send unsolicited or malicious content, we consider each request carefully\. If we can, we will grant your request within this 24\-hour period\. However, if we need additional information from you, it might take longer to resolve your request\.

If your use case doesn't align with our policies, we might be unable to grant your request\.

## Step 2: Update your SMS settings on the Amazon SNS console<a name="channels-sms-awssupport-spend-threshold-settings"></a>

After we notify you that your monthly spending quota has been increased, you have to adjust the spending quota for your account on the Amazon SNS console\.

**Important**  
 Important: If you skip this step, your SMS spend limit won't increase\.

**To adjust your spending quota on the console**

1. Open the Amazon SNS console\.

1. Open the left navigation menu, expand Mobile, and then choose Text messaging \(SMS\)\.

1. On the Mobile text messaging \(SMS\) page, next to Text messaging preferences, choose Edit\.

1. On the Edit text messaging preferences page, under Details, enter your new SMS spend limit for Account spend limit\.
**Note**  
You might receive a warning that the entered value is larger than the default spend limit\. You can ignore this\. 

1. Choose Save changes\.
**Note**  
If you get an "Invalid Parameter" error, check the contact from AWS Support and confirm that you entered the correct new SMS spend limit\. If you still experience a problem, open a case in the AWS Support Center\. 