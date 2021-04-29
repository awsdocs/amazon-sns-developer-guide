# Registering a company<a name="sns-settings-register-company"></a>

Before you can request a 10DLC, you need to register your company with The Campaign Registry\. 

**Note**  
Amazon SNS SMS messaging is available in Regions where Amazon Pinpoint is not currently supported\. In these cases, open the Amazon Pinpoint console in the US East \(N\. Virginia\) Region to register your 10DLC company and campaign, but *do not* request a 10DLC number\. Instead, open a case in the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to request a 10DLC number in the AWS Region where you use or plan to use Amazon SNS to send SMS messages\. For information on Regions where Amazon Pinpoint is available, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) in the *AWS General Reference*\.

## Register the company<a name="sns-register-company-steps-10dlc"></a>

You need to register your company only once\. After it's registered, you won't be able to make any direct changes to company information other than through filing a support request, described later in this section\.

**Important**  
You can only register a company in a single AWS account\. If you have multiple accounts, such as Test, Development, and Production accounts, you should only register in the Production account\. You can then use cross\-account access to send messages through the 10DLC phone number in your other accounts\. Do not enter fictitious or placeholder information in the registration process\. For more information on setting up cross\-account access, see [10DLC cross\-account access](sns-settings-sms-crossaccount-10dlc.md)\.

**To register a company**

The first step before using a 10DLC is to register your company and provide contact information\. You do this using the Amazon Pinpoint \(not Amazon SNS\) console\.

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **Account Settings**, and then under **SMS and voice**, choose **10DLC campaigns**\.

1. Choose **Register company**\. 

1. On the **Register your company** page, you'll see **Registration fee**\. This is a one\-time fee associated with registering your company\. This cost is separate from any other monthly costs or fees, and is charged to you only when your company has been registered\.

1. In the **Company info** section, provide the following information:
   + **Legal company name** – This is the name under which the company is registered\. You must use the company's legal name\. Do not modify or change this name\.
   + **What type of legal form is this organization** – Choose one of the following from the dropdown list: **Private profit**, **Public for profit**, or **Not for profit**\.
   + If you choose **Public for profit**, you're prompted to supply the company's stock symbol and the stock exchange on which it's listed\.
   + Choose the country where the company is registered from the **Country of registration** list\.
   + If the company is known by any name other than its registered name, enter that name in the **Doing Business As \(DBA\) or brand name** field\. 
   + Enter the business's **Tax ID**\. In the United States, this is a 9\-digit number\. A tax ID is required, so you must provide an actual tax ID\. Do not leave this blank or complete with false information\.
   + For **Vertical**, choose the vertical market that most closely resembles the company you're registering\.

1. In the **Contact info** section, provide contact information for contacting the company\.
   + **Address/Street** – The physical street address\. 
   + **City** – The city in which the physical address is located\.
   + **State or region** – The state or region where the address is located\.
   + **Zip Code /Postal Code** – The associated ZIP or postal code for the address\.
   + **Company website** – Provide the full URL, including HTTP or HTTPS\.
   + **Support email** – The full email address where your company support can be contacted\.
   + **Support phone number** – The phone number, using an E\.164 format, where your company support can be contacted\. For example, *12065550101*\. You don't need to supply the leading \+\.

1. When you're finished, choose **Create**\. This submits your company registration to The Campaign Registry\. Typically, approval is instantaneous\.

1. After your company is registered, you can create a 10DLC campaign\. For information about creating a 10DLC campaign, see [Registering a 10DLC campaign](sns-settings-register-campaign-10dlc.md)\.

## Editing or deleting a company<a name="sns-edit-company-10dlc"></a>

After a company is registered, you can't change or delete it except through filing a support request\. 

**To edit or delete a company**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **Account Settings**, and then under **SMS and voice**, choose **10DLC campaigns**\.

1. **To open the **Company info** page**, choose the link of the company you want to modify\.
**Note**  
On the **Company info** page you can also create a new 10DLC campaign\.

1. To open Support Center, choose **Request to delete/edit**\.

1. On the Support page, choose **Create case**\.

1. For the case type, choose **Service limit increase**\.

1. For the **Limit type**, choose **Pinpoint**\.

1. In the Requests section, choose the **Region**, and then for the **Limit**, choose **10DLC**\.

1. For **Use case description**, enter the company name you want to delete or edit, and then provide details for what you want to be done\.

1. Under **Contact options**, for **Preferred contact language**, choose the language that you prefer to use when communicating with the AWS Support team\.

1. For **Contact method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.