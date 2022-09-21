# Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS<a name="channels-sms-awssupport-long-code"></a>

**Important**  
Effective June 1, 2021, US telecom providers no longer support using person\-to\-person \(P2P\) long codes for application\-to\-person \(A2P\) communications to US destinations\. Instead, you need to use another type of origination ID for these messages\. For more information, see [Special requirements for sending SMS messages to US destinations](channels-sms-us-requirements.md)\.

To request [10DLC numbers](channels-sms-originating-identities-10dlc.md), [toll\-free numbers](channels-sms-originating-identities-tfn.md), and [P2P long codes](channels-sms-originating-identities-long-codes.md), use the Amazon Pinpoint console\. For detailed instructions, see [Requesting a number](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-request-number.html) in the *Amazon Pinpoint User Guide*\.

**Important**  
US mobile carriers have recently changed their regulations, and will require that all toll\-free numbers \(TFNs\) complete a registration process with a regulatory body before September 30, 2022\. For more information about registering a toll\-free number, see [Toll\-free number registration requirements and process](channels-sns-sms-tfn-register.md)\.  
If you purchased your toll\-free number on or before September 30, 2022 its status will be in the **Active** state until October 1, 2022, unless you've completed your registration and it has been returned with the state set to **Completed**\. Otherwise, it will be placed in the **Pending** state and you will not be able to send messages with it until you've register the number, the registration has been returned or the registration is set to the **Active** state\.  
Registration can take up to 15 business days\.

Amazon SNS SMS messaging is available in Regions where Amazon Pinpoint is not currently supported\. In these cases, open the Amazon Pinpoint console in the US East \(N\. Virginia\) Region to register your 10DLC company and campaign, but *do not* request a 10DLC number\. Instead, use the [AWS Service Quotas console](https://us-east-1.console.aws.amazon.com/support/home?region=us-east-1&skipRegion=true#/case/create?issueType=service-limit-increase) to create a service limit increase case while requesting the 10DLC number for that Region\. For information on Regions where Amazon Pinpoint is available, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) in the *AWS General Reference*\.

**Note**  
If you're new to SMS messaging with Amazon SNS, you should also request a monthly SMS spending threshold that meets the expected demands of your SMS use case\. By default, your monthly spending threshold is $1\.00 \(USD\)\. For more information, see [Requesting increases to your monthly SMS spending quota for Amazon SNS](channels-sms-awssupport-spend-threshold.md)\. 