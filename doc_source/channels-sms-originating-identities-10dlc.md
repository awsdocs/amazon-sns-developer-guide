# 10DLC<a name="channels-sms-originating-identities-10dlc"></a>

US carriers no longer support using application\-to\-person \(A2P\) SMS messaging over local, unregistered long codes\. For high\-volume A2P SMS messaging, US carriers instead offer a new type of long code called 10\-digit long codes \(10DLC\)\. 10DLC is only used in the US\. Unlike toll\-free numbers, 10DLC supports both transactional *and promotional* messaging, and can include any US area code\.

To get started with 10DLC, first use Amazon Pinpoint to register for 10DLC:

1. Register your company using the [Amazon Pinpoint console](https://console.aws.amazon.com/pinpoint/)\. Once you successfully complete your registration with Amazon Pinpoint, you can start registering 10DLC campaigns\.

   For more information, see [Registering a company](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-register-company.html) in the *Amazon Pinpoint User Guide*\.
**Note**  
Amazon SNS SMS messaging is available in Regions where Amazon Pinpoint is not currently supported\. In these cases, open the Amazon Pinpoint console in the US East \(N\. Virginia\) Region to register your 10DLC company and campaign\. Then open a case in the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to request that the registered 10DLC number is moved to the Region of operation\. For information on Regions where Amazon Pinpoint is available, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) in the *AWS General Reference*\.

   There are registration and monthly fees associated with using 10DLC, such as registering your company and 10DLC campaign\. These are separate from any other monthly or AWS fees\. For more information about 10DLC fees, see the Amazon SNS [Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/) page\.

1. From Amazon Pinpoint, create and register a 10DLC campaign\.

   If you've previously registered your company with The Campaign Registry, they may quickly approve your 10DLC campaign\. If The Campaign Registry requires more information, approval can take a week or more\.

   For more information, see [Registering a 10DLC campaign](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-register-campaign-10dlc.html) in the *Amazon Pinpoint User Guide*\.

1. From Amazon Pinpoint, request a 10DLC number\.

   Your company and 10DLC campaign must be approved by The Campaign Registry before you can purchase a 10DLC number from AWS\. 

   For more information, see [Requesting a number ](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-request-number.html) in the *Amazon Pinpoint User Guide*\.

You can now use Amazon SNS to send SMS messages to US destinations using the 10DLC number that is associated with the 10DLC campaign\.

For more information on The Campaign Registry and 10DLC, see the [FAQ](https://www.campaignregistry.com/faq/) at [campaignregistry\.com](https://www.campaignregistry.com)\. 