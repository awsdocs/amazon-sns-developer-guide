# Requesting sender IDs for SMS messaging with Amazon SNS<a name="channels-sms-awssupport-sender-id"></a>

**Important**  
If you're new to SMS messaging with Amazon SNS, request a monthly SMS spending threshold that meets the expected demands of your SMS use case\. By default, your monthly spending threshold is $1\.00 \(USD\)\. You can request to increase your spending threshold in the same support case that includes your request for a sender ID\. Or, if you prefer, you can open a separate case\. For more information, see [Requesting increases to your monthly SMS spending quota for Amazon SNS](channels-sms-awssupport-spend-threshold.md)\.

In SMS messaging, a *sender ID* is a name that appears as the message sender on recipients' devices\. Sender IDs are a useful way to identify yourself to the recipients of your messages\.

Support for sender IDs varies by country\. For example, carriers in the United States don't support sender IDs at all, but carriers in India require senders to use sender IDs\. For a complete list of countries that support sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

**Important**  
Some countries require you to register sender IDs before you use them to send messages\. Depending on the country, this registration process might take several weeks\. The countries that require pre\-registered sender IDs are indicated in the table on the [Supported Countries](sns-supported-regions-countries.md) page\.  
If you have enterprise support and are registering multiple templates across multiple please raise the requests as per instructions below, and work with your Technical Account Manager to ensure that your on\-boarding experience is co\-ordinated\.

If you're sending messages to recipients in a country where sender IDs are supported, and that country doesn't require you to register your sender ID, you don't have to perform any additional steps\. You can start sending messages that include sender ID values immediately\.

You only need to complete the procedures on this page if you plan to send messages to a country where registration of sender IDs is required\.

## Step 1: Open an Amazon SNS SMS case<a name="channels-sms-awssupport-sender-id-open"></a>

If you plan to send messages to recipients a country where sender IDs are required, you can request a sender ID by creating a new case in the AWS Support Center\.

**Note**  
If you plan to send messages to recipients in a country where sender IDs are allowed but not required, you don't need to open a case in the Support Center\. You can start sending messages that use sender IDs immediately\. 

**To request a sender ID**

1. Sign in to the AWS Management Console at [https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. On the **Support** menu, choose **Support Center**\.

1. On the **My support cases** tab, choose **Create case**\.

1. Choose **Service quota increase**\.

1. Under **Case classification**, do the following:

   1. For **Quota type**, choose **Pinpoint SMS**\.

   1. For **Provide a link to the site or app which will be sending SMS messages**, identify the website or application where your audience members opt in to receive your SMS messages\.

   1. For **What type of messages do you plan to send**, choose the type of message that you plan to send using your sender ID:
      + **One Time Password** – Messages that provide passwords that your customers use to authenticate with your website or application\.
      + **Promotional** – Noncritical messages that promote your business or service, such as special offers or announcements\.
      + **Transactional** – Important informational messages that support customer transactions, such as order confirmations or account alerts\. Transactional messages must not contain promotional or marketing content\.

   1. For **Which countries do you plan to send messages to**, specify the countries where you want to register a sender ID\. Support for sender IDs and sender ID registration requirements vary by country\. For more information, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

      If the list of countries exceeds the number of characters allowed by this text box, you can instead list the countries in the **Case description** section\.

1. Under **Requests**, do the following:

   1. For **Resource Type**, choose **General Quotas**\.

   1. For **Quota**, choose **SenderID Registration**\.

   1. For **New quota value**, enter the number of sender IDs that you're requesting\. Typically, this value is **1**\.

1. Under **Case description**, for **Use case description**, provide the following information:
   + The sender ID that you want to register\.
   + The template that you plan to use for your SMS messages\.
   + The number of messages that you plan to send to each recipient per month\.
   + Information about how your customers opt in to receiving messages from you\.
   + The name of your company or organization\.
   + The address that's associated with your company or organization\.
   + The country where your company or organization is based\.
   + A phone number for your company or organization\.
   + The URL of the website for your company or organization\.

After we receive your request, we provide an initial response within 24 hours\. We might contact you to request additional information\. If we're able to provide you with a Sender ID, we send you an estimate of the amount of time that's required to provision it\. 

In order to prevent our systems from being used to send unsolicited or malicious content, we have to consider each request carefully\. We might not be able to grant your request if your use case doesn't align with our policies\.

## Step 2: Update your SMS settings in the Amazon SNS console<a name="channels-sms-awssupport-sender-id-settings"></a>

When we complete the process of obtaining your sender ID, we respond to your case\. When you receive this notification, complete the steps in this section to configure Amazon SNS to use your sender ID as the default sender ID for all messages sent using your account\. Alternatively, you can choose to specify which sender ID to use when [ publishing the message\.](https://docs.aws.amazon.com/sns/latest/dg/sms_publish-to-phone.html) 

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose Mobile, Text messaging \(SMS\)\.

1. On the **Mobile text messaging \(SMS\)** page, in the **Text messaging preferences** section, choose **Edit**\.

1. On the **Edit text messaging preferences page**, in the **Details** section, do the following:

1. For **Default sender ID **, enter the provided sender ID to be used as the default for all messages from your account\.

1. Choose **Save changes**\.

## Next steps<a name="channels-sms-awssupport-sender-id-next"></a>

You've registered a sender ID and updated your settings in the Amazon SNS console\. Now you can use Amazon SNS to send SMS messages with your sender ID\. SMS recipients in supported countries will see your sender ID as the message sender on their devices\. If a different sender ID is used when publishing messages, it will override the default ID configured here\.