# Registering a 10DLC campaign<a name="sns-settings-register-campaign-10dlc"></a>

A 10DLC campaign indicates how you'll be using the 10DLC number you're requesting\. You do this using the Amazon Pinpoint \(not Amazon SNS\) console\. Before you can create and register a 10DLC campaign, your company must be registered\. For information on registering a company, see [Registering a company](sns-settings-register-company.md)\.

On this page, you first provide the details about the company you're creating the 10DLC campaign for and then provide the use case details of the campaign itself\. The information on this page is then provided to The Campaign Registry for approval\.

## Step 1: Choose the company associated with the 10DLC campaign<a name="sns-register-campaign-10dlc"></a>

In this section, you'll choose the company you're creating the 10DLC campaign for and provide additional details\.

**Important**  
Carefully verify that all of the information that you enter is correct\. Once you submit a campaign registration, you can only edit it by opening a ticket with AWS Support\. Errors in the registration process could result in reduced throughput for your 10DLC phone numbers\.

**To create a 10DLC campaign**
**Note**  
For each campaign you're registering, there is a carrier registration fee that won't be charged until your campaign is approved\. 

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **Account Settings**, and then under **SMS and voice**, choose **10DLC campaigns**\.

1. On the **Create a 10DLC campaign** page, the **Fees** section displays the one\-time carrier 10DLC campaign registration fee, separate from any monthly fees charged by AWS, in addition to the monthly 10DLC campaign fee\. This fee charged is per campaign\.

1. For **Company name**, choose the company that you're creating this campaign for\. If you haven't already registered the company, you must do that first\. 

1. Enter a **10DLC campaign name**\.

1. For **Vertical**, choose the vertical market that most closely resembles your market\.

1.  In **Help message**, enter the message that your customers receive if they respond with a HELP keyword\.

1. In **Stop message**, enter the message a customer receives from you if they respond with the STOP keyword\.

## Step 2: Add the use case details<a name="sns-register-campaign-usecase"></a>

After you've provided the company information, you'll next supply the 10DLC campaign details\. 

**To provide use case details**

1. Choose an option for the use case\. 
   + **Standard** – The purpose tells the mobile operators how you'll be using the 10DLC phone number\. You'll be prompted to supply additional details about the proposed campaign\.
**Note**  
In some cases, for instance if you've been previously registered with the Campaign Registry, you might be automatically approved\. 
   + **Special** – A special campaign might be an emergency or a time\-sensitive announcement\. For example, this might be a public safety warning or a sweepstakes announcement\. Some of these use cases require additional documentation\. If you choose one of those use cases, you're prompted to create a support case to justify the request\.

1. For **Use case**, choose a use case that most closely resembles your campaign from the preset list of use cases\. The monthly fee for each use case appears next to the use case name\.

1. Enter at least one **Sample message**\. This is the sample message you plan to send to your customers\. This message must be the exact message you'll be sending\.

1. If your use case supports multiple scenarios, enter up to four additional sample messages\.

1. The **Campaign and content attributes** section defines the characteristics of your 10DLC campaign\. These are a series of **Yes** or **No** options indicating particular features of the campaign\. Some attributes are mandatory, so you can't change the default value\. These values appear grayed out\.
**Important**  
Make sure that the attributes you choose are applicable to your campaign\. In some cases, choosing an attribute that is not applicable to your campaign can negatively affect your throughput per second \(TPS\)\.
   + **Subscriber opt\-in** – Subscribers can opt in to receive messages about this campaign\.
   + **Subscriber opt\-out** – Subscribers can opt out of receiving messages about this campaign\.
   + **Subscriber help** – Subscribers can contact the message sender after sending the HELP keyword\.
   + **Number pooling** – This 10DLC campaign uses more than 50 phone numbers\. 
   + **Direct lending or loan arrangement ** – The campaign includes information about direct lending or other loan arrangements\.
   + **Embedded link ** – The 10DLC campaign includes an embedded link\. URL shorteners, such as Tinyurl or Bitly, are not allowed\.
   + **Embedded phone number** – The campaign includes an embedded phone number that is not a customer support number\. 
   + **Affiliate marketing** – The 10DLC campaign includes information from affiliate marketing\.
   + **Age\-gated content** – The 10DLC campaign includes age\-gated content as defined by carrier and Cellular Telecommunications and Internet Association \(CTIA\) guidelines\.

1. Choose **Create**\. The SMS and voice page opens\. A message appears indicating that your campaign was submitted and is under review\. You can see the status of your request on the **10DLC campaigns** tab\. You can check the status of your registration on the **10DLC** tab, which will be one of the following:
   + **Active** – Your 10DLC campaign was approved\. You can request a 10DLC and assign that number to your campaign\. See [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.
   + **Pending** – Approval for your 10DLC campaign has not yet been received by one or more carriers\. In some cases, approval might take one week or more\. If the status changes to any other status, the Amazon Pinpoint console reflects that change\. We don't notify you of status changes\.
   + **Rejected** – Your 10DLC campaign was rejected by one or more carriers\. To get more information, submit a support request that includes the campaign ID of the rejected campaign\. Amazon Pinpoint doesn't include rejection reasons on the console, and we don't notify you if your campaign is rejected\.
   + **Suspended** – One or more carriers suspended your 10DLC campaign\. To get more information, submit a support request that includes the campaign ID of the suspended campaign\. Amazon Pinpoint doesn't include suspension reasons on the console, and we don't notify you if your campaign is suspended\.

1. If your 10DLC is approved, you can request a 10DLC number to associate with that campaign\. For information about requesting a 10DLC number, see [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.

## Editing or deleting a 10DLC campaign<a name="sns-edit-campaign-10dlc"></a>

After a campaign is registered with the carriers, you can't change it or delete except through filing a support request\. When filing the request, you must provide the 10DLC **Campaign ID**\.

**Important**  
If you have any 10DLC numbers associated with a campaign you want to delete, first remove those numbers from the campaign\. You can submit this request at the same time you're deleting a campaign\. 

**To edit or delete a 10DLC campaign**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **Account Settings**, and then under **SMS and voice**, choose **10DLC campaigns**\.

1. Choose the 10DLC campaign you want to modify\.

1. To open Support Center, choose **Request to delete/edit**\.

1. On the support page, choose **Create case**\.

1. For the case type, choose **Service limit increase**\.

1. For the **Limit type**, choose **Pinpoint**\.

1. In the **Requests** section, choose the **Region**, and then for the **Limit**, choose **10DLC**\.

1. For **Use case description**, enter the campaign ID of the campaign you want to edit or delete\. We recommend that you edit or delete only one campaign per each support request\.

1. Under **Contact options**, for **Preferred contact language**, choose the language that you prefer to use when communicating with the AWS Support team\.

1. For **Contact method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.