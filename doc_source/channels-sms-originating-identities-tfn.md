# Toll\-free numbers<a name="channels-sms-originating-identities-tfn"></a>

A toll\-free number is a 10\-digit number that begins with one of the following area codes: 800, 888, 877, 866, 855, 844, or 833\. You can use toll\-free numbers to send transactional messages only\. 

To purchase toll\-free numbers, use the Amazon Pinpoint console\. No message template registration is required\. For more information, see [Requesting a number](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-request-number.html) in the *Amazon Pinpoint User Guide*\.

Currently, Amazon Pinpoint supports toll\-free numbers for both voice and SMS messages, but Amazon SNS supports SMS messaging only\. 

## Guidelines for using toll\-free numbers<a name="tfn-guidelines"></a>

When using a toll\-free number as an origination number, follow these guidelines:
+ Don't use shortened URLs created from third\-party URL shorteners, as these messages are more likely to be filtered as spam\.

  If you need to use a shortened URL, consider using a [10DLC number](channels-sms-originating-identities-10dlc.md) or [short code](channels-sms-originating-identities-short-codes.md)\. Using short codes and 10DLCs require that you register your message template, where you can specify a shortened URL\.
+ Be aware that keyword opt\-out \(STOP\) and opt\-in \(UNSTOP\) responses are set at the carrier level\. You can't modify these keywords or other any other keywords, or the messages that are sent when users reply with STOP and UNSTOP\.
+ Don't send the same or similar message contents using multiple toll\-free numbers\. Carriers call this practice *snowshoeing* or *number pooling* and target these messages for filtering\.
+ Because toll\-free numbers are subject to content filtering, be sure your messages don't contain any of the following types of restricted content:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/channels-sms-originating-identities-tfn.html)