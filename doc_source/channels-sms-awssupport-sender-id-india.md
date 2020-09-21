# Special requirements for sending SMS messages to recipients in India<a name="channels-sms-awssupport-sender-id-india"></a>

By default, when you send messages to recipients in India, Amazon SNS uses International Long Distance Operator \(ILDO\) connections to transmit those messages\. When recipients see a message that's sent over an ILDO connection, it appears to be sent from a random numeric ID\. Choosing a route type \(transactional or promotional\) is mandatory when sending domestic messages to India\. For a promotional message, choose the promotional route type as this uses a numeric sender ID\. For a transactional message, choose the transactional route as this uses a case\-sensitive alphanumeric sender ID\. Please note that your account cannot use both numeric sender IDs and alphanumeic sender IDs in the same account\. Maintain separate accounts for each ID type\. Additional content guidelines can be found at the [Vilpower website](https://www.vilpower.in)\.

If you prefer to use an alphabetic sender ID for your SMS messages, you have to send those messages over local routes rather than ILDO routes\. To send messages using local routes, you first have to register your use case and message templates with the Telecom Regulatory Authority of India \(TRAI\)\. These registration requirements were designed to reduce the number of unsolicited messages that Indian consumers receive, and to protect consumers from potentially harmful messages\. This registration process is managed by Vodafone India through its Vilpower service\.

**Note**  
The price for sending messages using local routes is shown on the [Amazon SNS Worldwide SMS Pricing](https://aws.amazon.com/sns/sms-pricing/) page\. The price for sending messages using ILDO connections is higher than the price for sending messages through local routes\. Currently, the price for sending ILDO messages is USD $0\.02171 per message\.

To complete the registration process, you have to provide the following information:
+ Your organization's Permanent Account Number \(PAN\)\.
+ Your organization's Tax Deduction Account Number \(TAN\)\.
+ Your organization's Goods and Services Tax Identification Number \(GSTIN\)\.
+ Your organization's Corporate Identity Number \(CIN\)\.
+ A letter of authorization that gives you the authority to register your organization with Vilpower\. The Vilpower website includes a template that you can download and modify to fit your needs\.

Vilpower charges a fee for completing the registration process\. Currently, this fee is ₹5900\.

**To register your organization with TRAI**

1. In a web browser, go to the Vilpower website at [https://www\.vilpower\.in](https://www.vilpower.in)\.

1. Choose **Signup** to create another account\. During the registration process, do the following:
   + When you're asked to specify the type of entity that you want to register as, choose **As Enterprise**\.
   + On the Select your Telemarketer page, for Telemarketer Name, start typing **Infobip** and then choose **Infobip Private Limited – ALL** from the dropdown list\.
   +  For **Enter Telemarketer ID**, enter **110200001152**\.
   + When prompted to provide your Header IDs, enter the sender IDs that you want to register\.
   + When prompted to provide your Content Templates, enter the message content that you plan to send to your recipients\. Include a template for every message that you plan to send\. 

1. Complete the steps at [Requesting sender IDs](channels-sms-awssupport-sender-id.md) to request a sender ID for India\. In your request, provide the Entity ID, Unique Header IDs, and Template IDs that you received during the registration process\.

1. Download and complete the information on this [spreadsheet](https://docs.aws.amazon.com/pinpoint/latest/userguide/samples/India_DLT_Template_Validation.zip), and add it to your support case\. This provides additional information to Amazon, allowing successful registration with downstream carriers\.

1. Repeat steps 3 & 4 if any information, such as a Unique Header ID and/or Template ID, has changed, or if you modify an already registered template\.