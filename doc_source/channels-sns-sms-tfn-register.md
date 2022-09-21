# Toll\-free number registration requirements and process<a name="channels-sns-sms-tfn-register"></a>

**Important**  
US mobile carriers have recently changed their regulations and require all toll\-free numbers \(TFNs\) to complete a registration process with a regulatory body before September 30, 2022\. Check the status of your TFN by going to [Toll\-free number registration status](#tfn-registration-status)\. For more information about registering your company, see [Registering your toll\-free number](#registering-tfn)\.  
Allow up to 15 business days for processing after submitting your registration\.  
**Update September 19, 2022**: Effective October 1, 2022, mobile carriers will apply the following industry\-wide thresholds for messaging sent over any **unregistered toll\-free number**:   
Daily limit: 2,000 messages
Weekly limit: 12,000 messages
Monthly limit: 25,000 messages
**We strongly encourage you to complete your registration as soon as possible\. These limits are subject to decreases over time as carriers continue to restrict unregistered traffic\. **

You can use toll\-free phone numbers \(TFNs\) to deliver Amazon SNS messages to recipients\. After you request a TFN, you can use it to register your company\. Each TFN requires a single specific use case to use with it\. For example, if you register a TFN to use for one\-time passwords, the TFN can only be used for sending one\-time passwords\.

**Important**  
A TFN can be revoked if it is used for any purpose other then its specified use case\.

For guidelines on using TFNs, as well as their advantages and disadvantages, see [Toll\-free numbers](channels-sms-originating-identities-tfn.md)\.

## Toll\-free number forbidden use cases<a name="tfn-forbidden-use-cases"></a>

Amazon SNS has limited ability to send messages in cases where the messages are blocked \(for example, use cases related to controlled substance, or phishing\), or when high levels of filtering are expected \(for example, high risk financial messages\)\. You may be unable to register TFNs associated with restricted content use cases defined in [Guidelines for using toll\-free numbers](channels-sms-originating-identities-tfn.md#tfn-guidelines)\.

## Registering your toll\-free number<a name="registering-tfn"></a>

After [purchasing a TFN](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-request-number.html#settings-sms-request-number-console), you need to register the number\.

**Topics**
+ [Self\-serve registration for toll\-free numbers in Amazon Pinpoint regions](#tfn-self-serve-registration)
+ [Manual form\-based registration process for toll\-free numbers in regions other than Amazon Pinpoint regions](#manual-form-registration)
+ [Toll\-free number registration status](#tfn-registration-status)
+ [Editing a toll\-free number registration](#editing-tfn-registration)
+ [Deleting a toll\-free number registration](#deleting-tfn-registration)
+ [Toll\-free number frequently asked questions](#tfn-faq)

### Self\-serve registration for toll\-free numbers in Amazon Pinpoint regions<a name="tfn-self-serve-registration"></a>

If you've requested the TFN in the [Amazon Pinpoint regions](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html), complete the company registration process directly in the [Amazon Pinpoint console](https://console.aws.amazon.com/pinpoint/)\. When registering your TFN, make sure the information is complete and accurate, or your registration can be rejected\. The information you enter should be an exact match for your company's corporate head quarters\. 

**To register a toll\-free number**

1. Before you can register, you have to purchase the **TFN** by following the directions in [Toll\-free numbers](channels-sms-originating-identities-tfn.md)\.

1. Sign in to the **AWS Management Console**, and open the **Amazon Pinpoint console** at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane under **SMS and voice**, choose **Phone numbers**\.

1. On the **Toll\-free registrations** tab, choose the **Toll\-free Phone number** you want to register, then choose **Create registration**\.

1. In the **Company Information** section, do the following:
   + **Company Name** – Enter the name of your company\.
   + **Company Website** – Enter the URL for your company's website\. 
   + **Address 1** – Enter the street address of your corporate headquarters\. 
   + **Address 2 \(Optional\)** – Enter suite number of your corporate headquarters\. 
   + **City** – Enter the city of your corporate headquarters\. 
   + **State** – Enter the state of your corporate headquarters\. 
   + **ZIP Code** – Enter the ZIP code of your corporate headquarters\. 
   + **Country** – Enter the two digit ISO country code\. 

1. In the ** Contact Information** section, do the following:
   + **First Name** – Enter the first name for the contact person of your business\.
   + **Last Name** – Enter the last name for the contact person of your business\. 
   + **Support Email** – Enter the email address for the contact person of your business\.
   + **Support Phone Number** – Enter the phone number for the contact person of your business\.

1. In the **Messaging Use Case** section, do the following:
   + **Monthly SMS Volume** – Select the number of SMS messages for each month\.
   + **Use Case Category** – Select one of the following use case types that the number will be used for: 
     + **Two\-factor authentication** – Use this for sending two\-factor authentication codes\.
     + **One\-time passwords** – Use this for sending a user a one\-time password\.
     + **Notifications** – Use this if you only intend to send your users important notifications\.
     + **Polling and surveys** – Use this to poll users on their preferences\.
     + **Info on demand** – Use this for sending users messages after they have sent a request\.
     + **Promotions and Marketing** – Use this if you only intend to send marketing messages to your users\.
     + **Other** – Use this if your use case doesn't fall into any other category\. Complete the **Use Case Details** field\.
   + **Use Case Details \(Optional\)** – Use this field to give more context to the selected **Use Case Category**\.
   + **Opt\-in Workflow Description** – Enter a description of how users will consent to receive SMS messages\. For example, did they give consent by filling out an online form on your website\. 
   + **Opt\-in Workflow File** – Upload an image of how users consent to receiving messages\. Supported file types are PNG and ZIP\. Upload multiple images using a single ZIP file\. The maximum file size is 400 KB\. Examples of opt\-in screenshots are as follows:
     + **Website opt\-in** – Provide screenshots of a webform where the client adds their number and agrees to receive messages\.
     + **Website Posting \(Support\)** – Provide where the number is advertised, and where the customer finds the number to text\.
     + **Keyword or QR Code Opt\-in** – Provide where the customer finds the keyword or QR code to opt\-in to these messages\.
     + **2FA/OTP** – Provide a screenshot of the opt\-in \(if applicable\)\. If it the opt\-in is verbal, provide a screenshot of the verbal opt\-in script\.
     + **Informational** – Provide a screenshot of a verbal consent workflow and the messaging content\.

1. In the **Messaging Samples** section, do the following:
   + **Message Sample 1** – Enter an example message of the SMS message body that will be sent to your end users\. 
   + **Message Sample 2 \(Optional\)** and **Message Sample 3 \(Optional\)** – Enter more example messages of the SMS message body that will be sent to your end users\.

1. Choose **Submit registration**\.

### Manual form\-based registration process for toll\-free numbers in regions other than Amazon Pinpoint regions<a name="manual-form-registration"></a>

1. Use the following example registration form to complete the required information in the TFN registration CSV file\. To download the CSV template, choose [US\_TFN\_Registration\.zip](https://e030cbef99d049799cd4c36a33e56afa.s3.amazonaws.com/US_TFN_Registration.zip)\. 

   Each registration request or use case can only have up to five TFNs\. If you believe you qualify for an exemption to this rule, provide a detailed explanation for consideration\. List all phone numbers associated with the registration or use case\.  
![\[Example toll-free registration form.\]](http://docs.aws.amazon.com/sns/latest/dg/images/SNS-toll-free-registration-form-1.png)![\[Example toll-free registration form.\]](http://docs.aws.amazon.com/sns/latest/dg/images/SNS-toll-free-registration-form-2.png)

1. Create a case with [AWS Support](https://console.aws.amazon.com/support/home#/)\. Attach your completed CSV file to the case, and submit the TFN registration request\.

**Key point to note**

1. Registrations can take up to two weeks to process once all the required information has been submitted\. If information is missing or incomplete, the registration process will be delayed\. If your registration is rejected, we'll help you find the reason why it was denied and suggest methods to improve your campaign so it can be registered\.

1. TFNs work well for transactional use cases such as multi\-factor authentication \(MFA\) where limited throughput is required\. Each TFN can send up to three text messages parts per second, and each customer account can have up to five TFNs\. If you're sending over 15 text messages parts per second but less than 100, we recommend you register one or more [10DLC](channels-sms-originating-identities-10dlc.md) origination IDs\. If your use cases require sending more than 100 text messages per second, we recommend you purchase and register one or more [short codes](channels-sms-originating-identities-short-codes.md)\. For more details, see [Guidelines for using toll\-free numbers](channels-sms-originating-identities-tfn.md#tfn-guidelines)\.

### Toll\-free number registration status<a name="tfn-registration-status"></a>

After you've registered a TFN, there are five different statuses that the registration can be in:
+ **Created** – Your registration has been created, but hasn't been submitted yet\.
+ **Reviewing** – Your registration has been accepted and is being reviewed\. It may take up to 15 business days for the review to be completed\.
+ **Complete** – Your registration has been approved and you can start using the TFN to start sending messages\.
+ **Requires Updates** – You need to fix your registration and resubmit it by following the directions in [Editing a toll\-free number registration](#editing-tfn-registration)\. Fields that require updates will have a warning icon and a brief description of the issue\.
+ **Closed** – You deleted the TFN and need to delete the registration for the number\.

**Check your registration status**

1. Open the **Amazon Pinpoint** console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **Toll\-free registrations** tab, choose the **Toll\-free Phone number**\.

1. You can view the registration status of each TFN\.

### Editing a toll\-free number registration<a name="editing-tfn-registration"></a>

If there is an issue with the registration after you've submitted it, the **registration status** will be set to **Requires Updates**\. You can make changes to the registration form in this state\. Fields that require updates will have a warning icon and a brief description of the issue\.

  **To edit a company registration**   Open the **Amazon Pinpoint** console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.   In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.   On the **Toll\-free registrations** tab, choose the **Toll\-free Phone number** you want to edit, then select the **Registration ID**\.   Registrations fields that need to be updated have a warning icon\. Select the **Update registration** button to edit the form\.    Reupload any previously submitted files for **Opt\-in Screenshot file**\. Use the **Opt\-in Screenshot file** to upload an image of how users consent to receiving messages\. Supported file types are PNG and ZIP\. Upload multiple images using a single ZIP file\. The maximum file size is 400 KB\.    It is a best practice to verify all fields are correct, as only the first error might be flagged\.   Once you've completed updating the registration, you can resubmit it by selecting the **Submit registration** button\.    

### Deleting a toll\-free number registration<a name="deleting-tfn-registration"></a>

After you've submitted the TFN registration, you can delete the number\. It is recommend that you delete the TFN before the registration\.

**To delete a registration**

1. Open the **Amazon Pinpoint console** at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers** tabs\.

1. On the **Toll\-free registrations** tab, choose the **Registration ID** you want to delete, then select the **Delete Registration** button\.

### Toll\-free number frequently asked questions<a name="tfn-faq"></a>

Frequently asked questions about the TFN registration process\.

#### Do I currently own a toll\-free number?<a name="collapsible-question-1"></a>

**To check if you own a toll\-free number **
+ Open the **Amazon Pinpoint console** at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.
+ In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.
+ The TFN **type** is listed as **toll\-free**\.

#### Do I have to register my toll\-free number?<a name="collapsible-question-2"></a>

Yes\. To continue using a TFN you currently own, you must register it before September 30, 2022\. If you are purchasing a new TFN after September 30, 2022, you must register it before you can send messages\.

#### How do I purchase a toll\-free number?<a name="collapsible-question-3"></a>

Follow the directions at [ Requesting a phone number using the Amazon Pinpoint console](https://docs.aws.amazon.com/pinpoint/latest/userguide/settings-sms-request-number.html#settings-sms-request-number-console) to purchase a TFN\.

#### How do I register my toll\-free number?<a name="collapsible-question-4"></a>

Follow the directions at [Registering your toll\-free number](#registering-tfn) to register a TFN\.

#### What is the registration status of my toll\-free number and what does it mean?<a name="collapsible-question-5"></a>

Follow the directions at [Toll\-free number registration status](#tfn-registration-status) to check your registration and status\.

#### What information do I need to provide?<a name="collapsible-question-6"></a>

You need to provide your company's address, a business contact, and a use case for the TFN\. You can find the required information at [Registering your toll\-free number](#registering-tfn)\.

#### What if my registration is rejected?<a name="collapsible-question-7"></a>

If your registration is rejected, the status is changed to **Requires Updates**\. To make updates, see [Editing a toll\-free number registration](#editing-tfn-registration)\.

#### What permissions do I need?<a name="collapsible-question-8"></a>

The IAM user/role you use to visit the Amazon Pinpoint console must have the *“sms\-voice:\*”* permission, otherwise you'll get an access denied error\. 