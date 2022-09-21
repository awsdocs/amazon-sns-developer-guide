# Short codes<a name="channels-sms-originating-identities-short-codes"></a>

Short codes are numeric sequences that are shorter than a regular phone number\. For example, in the United States and Canada, standard phone numbers \(long codes\) contain 11 digits, while short codes contain five or six digits\. Amazon SNS supports dedicated short codes\.

## Dedicated short codes<a name="channels-sms-originating-identities-dedicated-short-codes"></a>

If you send a large volume of SMS messages to recipients in the United States or Canada, you can purchase a dedicated short code\. Unlike the short codes in the shared pool, dedicated short codes are reserved for your exclusive use\.

Using a memorable short code can help build trust\. If you need to send sensitive information, such as one\-time passwords, it's a good idea to send it using a short code so that your customer can quickly determine whether a message is actually from you\.

If you're running a new customer acquisition campaign, you can invite potential customers to send a keyword to your short code \(for example, “Text ‘FOOTBALL’ to 10987 for football news and information”\)\. Short codes are easier to remember than long codes, and it's easier for customers to enter short codes into their devices\. By reducing the amount of difficulty that customers encounter when they sign up for your marketing programs, you can increase the effectiveness of your campaigns\.

Because mobile carriers must approve new short codes before making them active, they are less likely to flag messages sent from short codes as unsolicited\.

When you use dedicated short codes to send SMS messages, you can send a higher volume of messages per 24\-hour period than you can when you use other types of origination identities\. In other words, you have a much higher *sending quota*\. You can also send a much higher volume of messages per second\. That is, you have a much higher *sending rate*\.

**Advantages**

Using a memorable short code can help build trust\. If you need to send sensitive information, such as one\-time passwords, it's a good idea to send it using a short code so that your customer can quickly determine whether a message is actually from you\.

If you're running a new customer acquisition campaign, you can invite potential customers to send a keyword to your short code \(for example, "Text FOOTBALL to 10987 for football news and information"\)\. Short codes are easier to remember than long codes, and it's easier for customers to enter short codes into their devices\. By reducing the amount of difficulty that customers encounter when they sign up for your marketing programs, you can increase the effectiveness of your campaigns\.

Because mobile carriers must approve new short codes before making them active, they are less likely to flag messages sent from short codes as unsolicited\.

When you use short codes to send SMS messages, you can send a higher volume of messages per 24\-hour period than you can when you use other types of originating identities\. In other words, you have a much higher *sending quota*\. You can also send a much higher volume of messages per second\. That is, you have a much higher *sending rate*\.

**Disadvantages**

There are additional costs to acquire short codes, and they can take a long time to implement\. For example, in the United States, there's a one\-time setup fee of $650\.00 \(USD\) for each short code, plus an additional recurring charge of $995\.00 per month for each short code\. It can take 8–12 weeks for short codes to become active on all carrier networks\. To find the price and provisioning time for a different country or region, complete the procedure described in [Requesting dedicated short codes for SMS messaging with Amazon SNS](channels-sms-awssupport-short-code.md)\.