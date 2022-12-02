# Sender IDs<a name="channels-sms-originating-identities-sender-ids"></a>

A sender ID is an alphabetic name that identifies the sender of an SMS message\. When you send an SMS message using a sender ID, and the recipient is in an area where sender ID authentication is supported, your sender ID appears on the recipient’s device instead of a phone number\. A sender ID provides SMS recipients with more information about the sender than a phone number, long code, or short code provides\.



Sender IDs are supported in several countries and regions around the world\. In some places, if you're a business that sends SMS messages to individual customers, you must use a sender ID that's pre\-registered with a regulatory agency or industry group\. For a complete list of countries and regions that support or require sender IDs, see [Supported Regions and countries](sns-supported-regions-countries.md)\.

There's no additional charge for using sender IDs\. However, support and requirements for sender ID authentication varies\. Several major markets \(including Canada, China, and the United States\) don't support using sender IDs\. Some areas require that companies who send SMS messages to individual customers must use a sender ID that's pre\-registered with a regulatory agency or industry group\.

**Important**  
AWS prohibits [SMS spoofing](https://en.wikipedia.org/wiki/SMS_spoofing), where the sender ID is used to impersonate another person, company, or product\. Only use a sender ID that represents a brand or trademark that you own\.

**Advantages**

Sender IDs provide the recipient with more information about the message sender\. It's easier to establish your brand identity by using a sender ID than by using a short or long code\. There's no additional charge for using a sender ID\. 

**Disadvantages**

Support and requirements for sender ID authentication aren't consistent across all countries or regions\. Several major markets \(including Canada, China, and the United States\) don't support sender ID\. In some areas, you must have your sender IDs pre\-approved by a regulatory agency before you can use them\.