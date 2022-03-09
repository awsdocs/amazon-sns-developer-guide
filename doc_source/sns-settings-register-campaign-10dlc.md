# Registering a 10DLC campaign<a name="sns-settings-register-campaign-10dlc"></a>

When you register a 10DLC campaign, you provide a description of your use case, as well as the message templates that you plan to use\. Before you can create and register a 10DLC campaign, you must first register your company\. For information on registering your company, see [Registering a company](sns-settings-register-company.md)\.

**Note**  
After you register your company, Amazon Pinpoint shows one of two statuses for the registration: **Verified** or **Unverified**\. You can only complete the 10DLC campaign registration process if the status of your company registration is **Verified**\. You will be able to create low\-volume mixed use campaigns\.  
If the status is **Unverified**, it usually means that some of the data that you provided when you registered your company was incorrect\. You won't be able to create any 10DLC campaigns while your company has this status\. You can modify your company registration to attempt to fix the issues with your company registration\. For more information about modifying 10DLC company registrations, see [Editing or deleting a registered company](sns-settings-10dlc-modify-company.md)\.

On this page, you first provide the details about the company you're creating the 10DLC campaign for and then provide the use case details of the campaign itself\. The information on this page is then provided to The Campaign Registry for approval\.

In this section, you'll choose the company you're creating the 10DLC campaign for and provide additional details\.

**To register a 10DLC campaign**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, choose **Create a 10DLC campaign**\.

1. On the **Create a 10DLC campaign** page, in the **Campaign info** section, do the following:

   1. For **Company name**, choose the company that you're creating this campaign for\. If you haven't already registered the company, you must do that first\. For more information about registering a company, see [Registering a company](sns-settings-register-company.md)\.

   1. For **10DLC campaign name**, enter a name for the campaign\.

   1. For **Vertical**, choose option that best represents your company\.

   1. For **Help message**, enter the message that your customers receive if they send the keyword "HELP" to your 10DLC phone number\.

   1. For **Stop message**, enter the message that your customers receive if they send the keyword "STOP" to your 10DLC phone number\.
**Tip**  
Your customers can reply to your messages with the word "HELP" to learn more about the messages that they're receiving from you\. They can also reply "STOP" to opt\-out of receiving messages from you\. The US mobile carriers require you to provide responses to both of these keywords\.  
The following is an example of a HELP response that complies with the requirements of the US mobile carriers:  
**ExampleCorp Account Alerts: For help call 1\-888\-555\-0142 or go to example\.com\. Msg&data rates may apply\. Text STOP to cancel\.**  
The following is an example of a compliant STOP response:  
**You are unsubscribed from ExampleCorp Account Alerts\. No more messages will be sent\. Reply HELP for help or call 1\-888\-555\-0142\.**  
Your responses to these keywords must contain 160 characters or fewer\.

1. In the **Campaign use case** section, do the following:

   1. For **Use case type**, if you have a charity\-related use case, choose **Special**\. Otherwise, choose **Standard**\.

   1. For **Use case**, choose a use case that most closely resembles your campaign from the preset list of use cases\. The monthly fee for each use case appears next to the use case name\.
**Note**  
The monthly charge for registering the 10DLC campaign is shown next to each use case type\. Most 10DLC campaign types have the same monthly charge\. The charge for registering Low\-Volume Mixed use cases is lower than for other use case types\. However, Low\-Volume Mixed campaigns support lower throughput rates than other campaign types\.

   1. Enter at least one **Sample SMS message**\. This is the sample message you plan to send to your customers\. If you plan to use multiple message templates for this 10DLC campaign, include them as well\.
**Important**  
Don't use placeholder text for your sample messages\. The example messages that you provide should reflect the actual messages that you plan to send as accurately as possible\.

1. The **Campaign and content attributes** section contains a series of **Yes** or **No** questions related to the particular features of the campaign\. Some attributes are mandatory, so you can't change the default value\.

   Make sure that the attributes you choose are accurate for your campaign\.

   Indicate whether each of the following applies to the campaign that you're registering:
   + **Subscriber opt\-in** – Subscribers can opt in to receive messages about this campaign\.
   + **Subscriber opt\-out** – Subscribers can opt out of receiving messages about this campaign\.
   + **Subscriber help** – Subscribers can contact the message sender after sending the HELP keyword\.
   + **Number pooling** – This 10DLC campaign uses more than 50 phone numbers\. 
   + **Direct lending or loan arrangement ** – The campaign includes information about direct lending or other loan arrangements\.
   + **Embedded link ** – The 10DLC campaign includes an embedded link\. Links from common URL shorteners, such as TinyUrl or Bit\.ly, are not allowed\. However, you can use URL shorteners that offer custom domains\.
   + **Embedded phone number** – The campaign includes an embedded phone number that isn't a customer support number\. 
   + **Affiliate marketing** – The 10DLC campaign includes information from affiliate marketing\.
   + **Age\-gated content** – The 10DLC campaign includes age\-gated content as defined by carrier and Cellular Telecommunications and Internet Association \(CTIA\) guidelines\.

1. Choose **Create**\. 

   After you submit the registration details for your campaign, the SMS and voice page opens\. A message appears indicating that your campaign was submitted and is under review\. You can see the status of your request on the **10DLC campaigns** tab\. You can check the status of your registration on the **10DLC** tab, which will be one of the following:
   + **Active** – Your 10DLC campaign was approved\. You can request a 10DLC phone number and associate that number with your campaign\. For more information, see [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.
   + **Pending** – Your 10DLC campaign hasn't been approved yet\. In some cases, approval might take one week or more\. If the status changes, the Amazon Pinpoint console reflects that change\. We don't notify you of status changes\.
   + **Rejected** – Your 10DLC campaign was rejected\. To get more information, submit a support request that includes the campaign ID of the rejected campaign\.
   + **Suspended** – One or more carriers suspended your 10DLC campaign\. To get more information, submit a support request that includes the campaign ID of the suspended campaign\. Amazon Pinpoint doesn't include suspension reasons on the console, and we don't notify you if your campaign is suspended\.

1. If your 10DLC is approved, you can request a 10DLC number to associate with that campaign\. For information about requesting a 10DLC number, see [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.

## Using 10DLC campaigns in multiple AWS Regions<a name="sns-settings-10dlc-multiple-regions"></a>

When you register a company, that company is available in your AWS account in all AWS Regions\. However, the same isn't true for 10DLC campaigns\. A 10DLC campaign can only be used in the AWS Region that it was registered in\.

If you plan to use 10DLC in more than one AWS Region, you must register separate 10DLC campaigns in each of those Regions\. This step is necessary in order to comply with carrier requirements\. You're charged for each campaign that you register, even if the use case is exactly the same\.

Registering multiple campaigns has the added benefit of increasing your throughput rates for messages that you send to recipients who use AT&T as their mobile carrier, because AT&T provides 10DLC throughput rates for each campaign\. Compare this to the way that T\-Mobile handles 10DLC throughput, which is based on a daily message allocation for each company \(regardless of the number of campaigns\)\.