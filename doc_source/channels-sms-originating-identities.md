# Originating identities for SMS messages<a name="channels-sms-originating-identities"></a>

When you send SMS messages using Amazon SNS, you can identify yourself to your recipients in one of three ways: by using a sender ID, by using a long code, or by using a short code\. These methods of identifying yourself to your customers are known as *originating identities*\. Each of these types of originating identities has its own advantages and disadvantages, which are discussed in the following sections\.

## Sender IDs<a name="channels-sms-originating-identities-sender-ids"></a>

A sender ID is an alphabetic name that identifies the sender of an SMS message\. When you send an SMS message using a sender ID, and the recipient is in an area where sender ID authentication is supported, your sender ID appears on the recipient’s device instead of a phone number\. A sender ID provides SMS recipients with more information about the sender than a phone number or short code provides\.

Sender IDs are supported in several countries and regions around the world\. In some places, if you're a business that sends SMS messages to individual customers, you must use a sender ID that's pre\-registered with a regulatory agency or industry group\. 

**Advantages**

Sender IDs provide the recipient with more information about the message sender\. It's easier to establish your brand identity by using a sender ID than by using a short or long code\. There's no additional charge for using a sender ID\. 

**Disadvantages**

Support and requirements for sender ID authentication aren't consistent across all countries or regions\. Several major markets \(including Canada, China, and the United States\) don't support sender ID\. In some areas, you must have your sender IDs pre\-approved by a regulatory agency before you can use them\.

## Long codes<a name="channels-sms-originating-identities-long-codes"></a>

Long codes are phone numbers that use the number format of the country or region where your recipients are located\. Long codes are also referred to as long numbers or virtual mobile numbers\. For example, in the United States and Canada, long codes contain 11 digits: the number 1 \(the country code\), a three\-digit area code, and a seven\-digit phone number\.

If you're using the two\-way SMS feature to send and receive SMS messages, you can request up to five dedicated long codes per country\. For more information about requesting long codes, see [Requesting dedicated long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.

**Advantages**

Dedicated long codes are reserved for use by your Amazon SNS account only—they aren't shared with other users\. When you use dedicated long codes, you can specify which long code you want to use when you send each message\. If you send multiple messages to the same customer, you can ensure that each message appears to be sent from the same phone number\. For this reason, dedicated long codes can be helpful in establishing your brand or identity\.

**Disadvantages**

If you send several hundred messages per day from a dedicated long code, mobile carriers might identify your number as one that sends unsolicited messages\. If your long code is flagged, your messages might not be delivered to your recipients\.

Long codes also have limited throughput\. In the United States and Canada, where long codes are most commonly used, you can send a maximum of one message per second\. \(The maximum sending rates for other countries vary\. Contact AWS Support for more information\)\. If you plan to send large volumes of SMS messages, or you plan to send at a rate greater than one message per second, you should purchase a dedicated short code\.

Many jurisdictions have restrictions related to using long codes to send Application\-to\-Person \(A2P\) SMS messages\. An A2P SMS is a message that's sent to a customer's mobile device when that customer submits his or her mobile number to an application\. A2P messages are one\-way conversations, such as marketing messages, one\-time passwords, and appointment reminders\. If you plan to send A2P messages, you should purchase a dedicated short code \(if your customers are in the United States or Canada\), or use a sender ID \(if your recipients are in a country or region where sender IDs are supported\)\.

## Short codes<a name="channels-sms-originating-identities-short-codes"></a>

Short codes are numeric sequences that are shorter than a regular phone number\. For example, in the United States and Canada, standard phone numbers \(long codes\) contain 11 digits, while short codes contain five or six digits\. Amazon SNS supports dedicated short codes\.

### Dedicated short codes<a name="channels-sms-originating-identities-dedicated-short-codes"></a>

If you send a large volume of SMS messages to recipients in the United States or Canada, you can purchase a dedicated short code\. Unlike the short codes in the shared pool, dedicated short codes are reserved for your exclusive use\.

**Advantages**

Using a memorable short code can help build trust\. If you need to send sensitive information, such as one\-time passwords, it's a good idea to send it using a short code so that your customer can quickly determine whether a message is actually from you\.

If you're running a new customer acquisition campaign, you can invite potential customers to send a keyword to your short code \(for example, “Text ‘FOOTBALL’ to 10987 for football news and information”\)\. Short codes are easier to remember than long codes, and it's easier for customers to enter short codes into their devices\. By reducing the amount of difficulty that customers encounter when they sign up for your marketing programs, you can increase the effectiveness of your campaigns\.

Because mobile carriers must approve new short codes before making them active, they are less likely to flag messages sent from short codes as unsolicited\.

When you use dedicated short codes to send SMS messages, you can send a higher volume of messages per 24\-hour period than you can when you use other types of originating identities\. In other words, you have a much higher *sending quota*\. You can also send a much higher volume of messages per second\. That is, you have a much higher *sending rate*\.

**Important**

There are additional costs to acquire short codes, and they can take a long time to implement\. For example, in the United States, there's a one\-time setup fee of $650\.00 \(USD\) for each short code, plus an additional recurring charge of $995\.00 per month for each short code\. It can take 8–12 weeks for short codes to become active on all carrier networks\. To find the price and provisioning time for a different country or region, complete the procedure described in [Requesting dedicated short codes for SMS messaging with Amazon SNS](channels-sms-awssupport-short-code.md)\.