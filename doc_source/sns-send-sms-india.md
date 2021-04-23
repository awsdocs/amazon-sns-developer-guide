# Step 3: Sending SMS messages<a name="sns-send-sms-india"></a>

After [registering your organization with TRAI](sns-india-register-with-trai.md), you can send SMS messages to recipients in India\. 

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, set the region selector to a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Mobile Text messaging \(SMS\)** page, choose **Publish text message**\. The **Publish SMS message** window opens\.

1. For **Message type**, choose one of the following:
   + **Promotional** – Noncritical messages, such as marketing messages\. 

     When using numeric sender IDs, choose this option\.
   + **Transactional** – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\.

     When using alphabetic or alphanumeric sender IDs, choose this option\.

   This message\-level setting overrides your default message type, which you set on the **Text messaging preferences** page\.

   For pricing information for promotional and transactional messages, see [Global SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\.

1. For **Number**, type the phone number to which you want to send the message\.

1. For **Message**, type the message to send\.

   When adding content to SMS messages, make sure that it exactly matches the content in the DLT registered template\. Carriers block SMS messages if their message content includes additional character returns, spaces, punctuation, or mismatched sentence case\. Variables in a template can have 30 or fewer characters\. 

1. Expand the **Origination identities** section\. For the **Sender ID**, type a custom ID that contains 3\-11 characters\.

   Sender IDs can be numeric for promotional messages, or alphabetic or alphanumeric for transactional messages\. The sender ID is displayed as the message sender on the receiving device\.

1. Expand the **Country\-specific attributes** section and specify the following required attributes for sending SMS messages to recipients in India:
   + **Entity ID** – The entity ID or principal entity \(PE\) ID that you received from the regulatory body for sending SMS messages to recipients in India\.

     This is a custom, TRAI\-provided string of 1\-30 characters that uniquely identifies the entity that you registered with the TRAI\.
   + **Template ID** – The template ID that you received from the regulatory body for sending SMS messages to recipients in India\.

     This is a custom, TRAI\-provided string of 1\-30 characters that uniquely identifies the template that you registered with the TRAI\. The template ID must be associated with the sender ID that you specified in the previous step, and with the message content\. 

1. Choose **Publish message**\.

For information on sending SMS messages to recipients in other countries, see [Publishing to a mobile phone](sms_publish-to-phone.md)\.