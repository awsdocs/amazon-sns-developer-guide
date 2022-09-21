# Toll\-free numbers<a name="channels-sms-originating-identities-tfn"></a>

**Important**  
US mobile carriers have recently changed their regulations and require all toll\-free numbers \(TFNs\) to complete a registration process with a regulatory body before September 30, 2022\. Check the status of your TFN by going to [Toll\-free number registration status](channels-sns-sms-tfn-register.md#tfn-registration-status)\. For more information about registering your company see [Registering your toll\-free number](channels-sns-sms-tfn-register.md#registering-tfn)\.  
Allow up to 15 business days for processing after submitting your registration\.  
**Update September 19, 2022**: Effective October 1, 2022, mobile carriers will apply the following industry\-wide thresholds for messaging sent over any **unregistered toll\-free number**:   
 Daily limit: 2,000 messages 
 Weekly limit: 12,000 messages 
 Monthly limit: 25,000 messages 
**We strongly encourage you to complete your registration as soon as possible\. These limits are subject to decreases over time as carriers continue to restrict unregistered traffic\. **

A toll\-free number \(TFN\) is a 10\-digit number that begins with one of the following area codes: 800, 888, 877, 866, 855, 844, or 833\. You can use TFNs to send transactional messages only\. 

**Topics**
+ [Purchase a toll\-free number](#purchase-tfn)
+ [Register a toll\-free number](#register-tfn)
+ [Guidelines for using toll\-free numbers](#tfn-guidelines)
+ [Advantages and disadvantages of toll\-free numbers](#advantages-disadvantages-tfn)

## Purchase a toll\-free number<a name="purchase-tfn"></a>

To purchase TFNs, use the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\. For more information, see [Toll\-free number registration requirements and process](channels-sns-sms-tfn-register.md)\.

Currently, Amazon Pinpoint supports toll\-free numbers for both voice and SMS messages\. Amazon SNS supports SMS messaging only\. 

## Register a toll\-free number<a name="register-tfn"></a>

To register a TFN, see [Toll\-free number registration requirements and process](channels-sns-sms-tfn-register.md)\.

## Guidelines for using toll\-free numbers<a name="tfn-guidelines"></a>

TFNs are typically used only within the US for transactional messaging, such as registration confirmation or for sending one\-time passwords\. They can be used for both voice messaging and SMS\. Average throughput is three message parts per second \(MPS\)\. However, this throughput is affected by character encoding\. For more information about how character encoding affects message parts, see [SMS character limits in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-limitations-characters.html)\. For more information about registering a TFN, see [Toll\-free number registration requirements and process](channels-sns-sms-tfn-register.md)\.

Each customer account can have up to five TFNs\. If you're sending over 15 text messages per second but less than 100, we recommend that you register one or more [10DLC origination IDs](sns-settings-register-company.md#sns-register-company-steps-10dlc)\. If your use cases require sending more than 100 text messages per second, we recommend that you purchase and register one or more [short codes](channels-sms-originating-identities-short-codes.md)\.

When using a TFN as an origination number, follow these guidelines:
+ Don't use shortened URLs created from third\-party URL shorteners, as these messages are more likely to be filtered as spam\.

  If you need to use a shortened URL, consider using a [10DLC number](channels-sms-originating-identities-10dlc.md) or [short code](channels-sms-originating-identities-short-codes.md)\. Using short codes and 10DLCs require that you register your message template, where you can specify a shortened URL\.
+ Be aware that keywords opt\-out \(STOP\) and opt\-in \(UNSTOP\) responses are set at the carrier level\. You can't modify these keywords or other any other keywords\. You also can't modify messages that are sent when users reply with STOP and UNSTOP\.
+ Don't send the same or similar message contents using multiple TFNs\. Carriers call this practice *snowshoeing* or *number pooling* and target these messages for filtering\.
+ Because TFNs are subject to content filtering, be sure your messages don't contain any of the following types of restricted content:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/channels-sms-originating-identities-tfn.html)

## Advantages and disadvantages of toll\-free numbers<a name="advantages-disadvantages-tfn"></a>

**Advantages**

Toll\-free originators have higher MPS over long codes as well as good deliverability\.

**Disadvantages**

There's no control over opt\-outs and opt\-ins, as these are managed at the carrier level\. 

Do not include shortened URLs in your message, or use the number to send a promotional message\. Instead use a 10DLC number or a short code\. When you use a short code or 10DLC number, you need to register your message templates, which can contain shortened URLs and can be promotional messages\. For more about short codes, see [Short codes](channels-sms-originating-identities-short-codes.md)\. For more about 10DLC, see [10DLC](channels-sms-originating-identities-10dlc.md)\.