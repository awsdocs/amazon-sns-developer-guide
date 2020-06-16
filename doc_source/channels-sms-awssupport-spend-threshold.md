# Requesting increases to your monthly SMS spending quota for Amazon SNS<a name="channels-sms-awssupport-spend-threshold"></a>

Your spending quota determines how much money you can spend sending SMS messages through Amazon SNS each month\. When Amazon SNS determines that sending an SMS message would incur a cost that exceeds your spending quota for the current month, it stops publishing SMS messages within minutes\.

**Important**  
Because Amazon SNS is a distributed system, it stops sending SMS messages within minutes of the spending quota being exceeded\. During this period, if you continue to send SMS messages, you might incur costs that exceed your quota\.

We set the spending quota for all new accounts at $1\.00 \(USD\) per month\. This quota is intended to let you test the message\-sending capabilities of Amazon SNS\. This quota also helps to reduce the risk of sending large campaigns before you're actually ready to use Amazon SNS for your production workloads\. Finally, this quota is necessary to prevent malicious users from abusing Amazon SNS\.

To request an increase to the SMS spending quota for your account, open a quota increase case in the AWS Support Center\.

## Step 1: Open an Amazon SNS SMS case<a name="channels-sms-awssupport-spend-threshold-open"></a>

You can request an increase to your monthly spending quota by opening a quota increase case in the AWS Support Center\.

**Note**  
Some of the fields on the request form are marked as "optional\." However, AWS Support requires all of the information that's mentioned in the following steps in order to process your request\. If you don't provide all of the required information, you may experience delays in processing your request\.

**To request a spending quota increase**

1. Go to the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\. 

1. Choose **Create case**\.

1. Choose **Service limit increase**, and then under **Case classification**, perform the following steps:

1. For **Limit** type, choose **SNS Text Messaging**\.

1. For **Link to site or app which will be sending SMS \- optional**, enter the URL of your website or application\.

1. For **Type of messages \- optional**, choose **One Time Password**, **Promotional**, or **Transactional**, depending on what you plan to send\.

1. For **Targeted Countries** \- optional, choose **General Limits**\.

1. Under **Requests**, for Request 1, do the following:

1. For **Resource Type**, choose **General Limits**\.

1. For **New limit**, enter the needed spend limit that you calculated earlier\.

1. Under **Case description**, for Use case description, enter the description that you wrote earlier\.

1. Expand **Contact options**, and then choose your preferred contact language\.

1. Choose **Submit**\.

1. When you finish, choose **Submit**\. 

The AWS Support team provides an initial response to your request within 24 hours\.

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. If we’re able to do so, we'll grant your request within this 24\-hour period\. However, if we need to obtain additional information from you, it might take longer to resolve your request\.

We might not be able to grant your request if your use case doesn’t align with our policies\.

## Step 2: Update your SMS settings on the Amazon Pinpoint console<a name="channels-sms-awssupport-spend-threshold-settings"></a>

After we notify you that your monthly spending quota has been increased, you have to adjust the spending quota for your account on the Amazon SNS console\.

**Important**  
 Important: If you skip this step, your SMS spend limit won't increase\.

**To adjust your spending quota on the console**

1. Open the Amazon SNS console\.

1. Open the left navigation menu, expand Mobile, and then choose Text messaging \(SMS\)\.

1. On the Mobile text messaging \(SMS\) page, next to Text messaging preferences, choose Edit\.

1. On the Edit text messaging preferences page, under Details, enter your new SMS spend limit for Account spend limit \- 

1. Note: You might receive a warning that the entered value is larger than the default spend limit\. You can ignore this 

1. Choose Save changes\.

    If you get an "Invalid Parameter" error, check the contact from AWS Support and confirm that you entered the correct new SMS spend limit\. If you still experience a problem, open a case in the AWS Support Center\. 

1. Open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. On the **All projects** page, choose a project that uses the SMS channel\.

1. In the navigation pane, under **Settings**, choose **SMS and voice**\.

1. In the **SMS and voice** section, choose **Edit**\.

1. Under **Account\-level settings**, for **Account spending limit**, enter the maximum amount, in US Dollars, that you want to spend on SMS messages each calendar month\. You can specify a value that's less than or equal to the total monthly spending quota provided by AWS Support\. By setting a lower value, you can control your monthly spending while still retaining the capacity to scale up if necessary\.

1. Choose **Save changes**\.