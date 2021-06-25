# 10DLC<a name="channels-sms-originating-identities-10dlc"></a>

US carriers no longer support using application\-to\-person \(A2P\) SMS messaging over local, unregistered long codes\. For high\-volume A2P SMS messaging, US carriers instead offer a new type of long code called 10\-digit long codes \(10DLC\)\.

## What is 10DLC?<a name="sns-what-is-10dlc"></a>

10DLC is a type of long code that is registered with carriers to support high volume A2P SMS messaging using the 10\-digit phone number format\. Amazon SNS no longer offers local long codes as an SMS product and instead offers 10DLC\. 10DLC doesn't impact you if you use only short codes and toll\-free numbers\.

10DLC is a 10\-digit phone number used only in the United States\. Messages sent from a 10DLC to recipients show a 10\-digit number as the sender\. Unlike toll\-free numbers, 10DLC supports both transactional *and promotional* messaging, and can include any US area code\.

If you have existing local long codes, you can request that their local long codes be enabled for 10DLC\. To do so, complete the 10DLC registration process and then submit a support ticket\. In the event that there's a problem with enabling your long code for 10DLC, you're notified and instructed to request a new 10DLC through the Amazon Pinpoint \(not Amazon SNS\) console\. For information about how to file a support ticket to convert a long code, see [Associating a long code with a 10DLC campaign](sns-settings-associate-long-code-10dlc.md)\.

In order to use a 10DLC number, first register your company and create a 10DLC campaign using the Amazon Pinpoint \(not Amazon SNS\) console\. AWS shares this information with The Campaign Registry, a third party that approves or rejects your registration based on the information\. In some cases, registration occurs immediately\. For example, if you've previously registered with The Campaign Registry, they might already have your information\. However, some campaigns might take one week or longer for approval\. After your company and 10DLC campaign are approved, you can purchase a 10DLC number and associate it with your campaign\. Requesting a 10DLC might also take up to a week for approval\. Although you can associate multiple 10DLCs with a single campaign, you can't use the same 10DLC across multiple campaigns\. For each campaign you create, you need to have a unique 10DLC\. 

**Important**  
If you use local long codes or a shared pool of numbers, you must switch over to 10DLC\. There's no impact on you if you use only short codes and toll\-free numbers\. For more information about how The Campaign Registry uses your information, see their [FAQ](https://www.campaignregistry.com/faq/) at [campaignregistry\.com](https://www.campaignregistry.com)\.

## 10DLC capabilities<a name="sns-10dlc-capabilities"></a>

The capabilities of 10DLC phone numbers depend on the mobile carriers of your recipients\. AT&T provides a throughput limit based on the number of message parts that can be sent each minute\. T\-Mobile and Sprint provide a daily limit, with no limitation on the number of message parts that can be sent per minute\. As of February 15, 2021, Verizon hasn't announced its 10DLC throughput policy\.

When you set up 10DLC, you have to register your company\. Your company information is used by The Campaign Registry to calculate a trust score\. Your trust score determines the capabilities of your 10DLC phone number\. The following table shows the 10DLC trust score tiers and the capabilities of 10DLC numbers that fall into those tiers\. We're working with The Campaign Registry to provide additional information about finding the trust score for your company\.


| Tier | Message parts per minute \(AT&T\) | Maximum daily messages \(T\-Mobile\) | 
| --- | --- | --- | 
|  High  |  3,600  | 200,000 | 
| Medium\-high | 600 |  40,000  | 
| Medium\-low | 30 | 10,000 | 
| Basic | 12 | 2,000 | 

For a comparison of the different phone number types see [U\.S\. product number comparison](channels-sms-originating-identities-origination-numbers.md#sns-10dlc-product-comparison)\.

## Getting started with 10DLC<a name="sns-getting-started-with-10dlc"></a>

Use the Amazon Pinpoint \(not Amazon SNS\) console to request your 10DLC\. Follow these steps to set up 10DLC for use with your 10DLC campaigns\.

1. **Register your company\. **

   Before you can request a 10DLC, your company must be registered with The Campaign Registry; for information, see [Registering a company](sns-settings-register-company.md)\. Registration is typically instantaneous unless The Campaign Registry requires more information\. There is a one\-time registration fee to register your company, displayed on the registration page\. This one\-time fee is paid separately from your monthly charges for the campaign and 10DLC\. 
**Note**  
Amazon SNS SMS messaging is available in Regions where Amazon Pinpoint is not currently supported\. In these cases, open the Amazon Pinpoint console in the US East \(N\. Virginia\) Region to register your 10DLC company and campaign, but *do not* request a 10DLC number\. Instead, open a case in the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to request a 10DLC number in the AWS Region where you use or plan to use Amazon SNS to send SMS messages\. For information on Regions where Amazon Pinpoint is available, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) in the *AWS General Reference*\.

1. **Register your campaign\.**

   After your company is registered, create a 10DLC campaign and associate it with one of your registered companies\. This campaign is submitted to The Campaign Registry for approval\. In most cases, 10DLC campaign approval is instantaneous unless The Campaign Registry requires more information\. For more information, see [Registering a 10DLC campaign](sns-settings-register-campaign-10dlc.md)\. 

1. **Request your 10DLC number\.**

   After your 10DLC campaign is approved, you can request a 10DLC and associate that number with the approved campaign\. Your 10DLC campaign can only use a number approved for it\. See [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\. 

## 10DLC registration and monthly fees<a name="sns-10dlc-fees"></a>

There are registration and monthly fees associated with using 10DLC, such as registering your company and 10DLC campaign\. These are separate from any other monthly fees charged by AWS\. For more information, see the [Amazon SNS Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/) page\.