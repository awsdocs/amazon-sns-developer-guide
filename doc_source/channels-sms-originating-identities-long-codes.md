# Person\-to\-person \(P2P\) long codes<a name="channels-sms-originating-identities-long-codes"></a>

**Important**  
Effective April 1, 2021, US telecom providers no longer support using person\-to\-person \(P2P\) long codes for application\-to\-person \(A2P\) communications to US destinations\. Instead, you need to use another type of origination ID for these messages\. For more information, see [Special requirements for sending SMS messages to US destinations](channels-sms-us-requirements.md)\.

P2P long codes are phone numbers that use the number format of the country or region where your recipients are located\. P2P long codes are also referred to as long numbers or virtual mobile numbers\. For example, in the United States and Canada, P2P long codes contain 11 digits: the number 1 \(the country code\), a three\-digit area code, and a seven\-digit phone number\.

For more information about requesting P2P long codes, see [Requesting 10DLC numbers, toll\-free numbers, and P2P long codes for SMS messaging with Amazon SNS](channels-sms-awssupport-long-code.md)\.

Dedicated P2P long codes are reserved for use by your Amazon SNS account only—they aren't shared with other users\. When you use dedicated P2P long codes, you can specify which P2P long code you want to use when you send each message\. If you send multiple messages to the same customer, you can ensure that each message appears to be sent from the same phone number\. For this reason, dedicated P2P long codes can be helpful in establishing your brand or identity\.

P2P long codes aren't supported for A2P communications to US destinations\. 

If you send several hundred messages per day from a dedicated P2P long code, mobile carriers might identify your number as one that sends unsolicited messages\. If your P2P long code is flagged, your messages might not be delivered to your recipients\.

P2P long codes also have limited throughput\. The maximum sending rates varies by country\. Contact AWS Support for more information\. If you plan to send large volumes of SMS messages, or you plan to send at a rate greater than one message per second, you should purchase a dedicated short code\.

Some carriers don't allow you to use P2P long codes to send A2P SMS messages, including in the US\. An A2P SMS is a message that's sent to a customer's mobile device when that customer submits his or her mobile number to an application\. A2P messages are one\-way conversations, such as marketing messages, one\-time passwords, and appointment reminders\. If you plan to send A2P messages, you should purchase a dedicated short code \(if your customers are in the United States or Canada\), or use a sender ID \(if your recipients are in a country or region where sender IDs are supported\)\.