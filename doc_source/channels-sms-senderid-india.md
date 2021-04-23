# Special requirements for sending SMS messages to recipients in India<a name="channels-sms-senderid-india"></a>

By default, when you send messages to recipients in India, Amazon SNS uses International Long Distance Operator \(ILDO\) connections to transmit those messages\. When recipients see a message that's sent over an ILDO connection, it appears to be sent from a random numeric ID\. 

**Note**  
The price for sending messages using local routes is shown on the [Amazon SNS Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/) page\. The price for sending messages using ILDO connections is higher than the price for sending messages through local routes\. Currently, the price for sending ILDO messages is USD $0\.02171 per message\.

If you prefer to use an alphabetic sender ID for your SMS messages, you have to send those messages over local routes rather than ILDO routes\. To send messages using local routes, you must first register your use case and message templates with the Telecom Regulatory Authority of India \(TRAI\) through Distributed Ledger Technology \(DLT\) portals\. These registration requirements are designed to reduce the number of unsolicited messages that Indian consumers receive and to protect consumers from potentially harmful messages\. This registration process is managed by Vodafone India through its Vilpower service\.

**Note**  
You can't use both numeric sender IDs and alphanumeric sender IDs in the same account\. If you use both ID types, you must maintain separate accounts for each\. For additional content guidelines, see the Vilpower website at [https://www\.vilpower\.in](https://www.vilpower.in)\.

**Topics**
+ [Sending SMS messages to India: task overview](sns-register-entity-and-template.md)
+ [Step 1: Registering with the TRAI](sns-india-register-with-trai.md)
+ [Step 2: Requesting a sender ID](sns-india-request-sender-id.md)
+ [Step 3: Sending SMS messages](sns-send-sms-india.md)
+ [Troubleshooting SMS messages sent to recipients in India](sns-send-sms-india-troubleshooting.md)