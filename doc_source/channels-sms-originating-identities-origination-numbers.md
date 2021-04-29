# Origination numbers<a name="channels-sms-originating-identities-origination-numbers"></a>

An origination number is a numeric string that identifies the SMS message sender's phone number\. When you send an SMS message using an origination number, the recipient's device shows the origination number as the sender's phone number\. You can specify different origination numbers by use case\.

Support for origination numbers is not available in countries where local laws require the use of sender IDs instead of origination numbers\. 

**Topics**
+ [10DLC](channels-sms-originating-identities-10dlc.md)
+ [Toll\-free numbers](channels-sms-originating-identities-tfn.md)
+ [Short codes](channels-sms-originating-identities-short-codes.md)
+ [Person\-to\-person \(P2P\) long codes](channels-sms-originating-identities-long-codes.md)
+ [U\.S\. product number comparison](#sns-10dlc-product-comparison)

## U\.S\. product number comparison<a name="sns-10dlc-product-comparison"></a>

This table shows the support comparison for U\.S\. phone number types\.


| Product feature | Short code | Toll\-free number | 10DLC | 
| --- | --- | --- | --- | 
| Number format | 5\-6 digits | 10\-digit number  | 10\-digit number  | 
| Channel support |  SMS  | SMS | SMS | 
| SMS traffic type | Promotional and transactional | Transactional | Promotional and transactional | 
| Requires vetting | Yes | No | Yes | 
| Estimated provisioning time | 12 weeks¹ | Immediately | 1 week | 
| SMS throughput \(number of SMS messages per second\)² | 100 message parts per second; higher throughput available for an additional fee\. | 3 message parts per second | Varies based on your 10DLC registration\. Supports up to 100 message parts per second\. | 
| Keywords required | Opt\-in, opt\-out, and HELP | STOP, UNSTOP\. These are network\-managed\. You cannot change the opt\-out and opt back in messages\. | Opt\-in, opt\-out, and HELP | 

¹ Provisioning estimate doesn't include approval time\.

² For more information on the maximum size for SMS messages, see [Publishing to a mobile phone](sms_publish-to-phone.md)\.