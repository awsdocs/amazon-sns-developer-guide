# Registering a company<a name="sns-settings-register-company"></a>

Before you can request a 10DLC, you need to register your company with The Campaign Registry\. 

**Note**  
Amazon SNS SMS messaging is available in Regions where Amazon Pinpoint is not currently supported\. In these cases, open the Amazon Pinpoint console in the US East \(N\. Virginia\) Region to register your 10DLC company and campaign, but *do not* request a 10DLC number\. Instead, open a case in the [AWS Support Center](https://console.aws.amazon.com/support/home#/) to request a 10DLC number in the AWS Region where you use or plan to use Amazon SNS to send SMS messages\. For information on Regions where Amazon Pinpoint is available, see [Amazon Pinpoint endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/pinpoint.html) in the *AWS General Reference*\.

## 10DLC company registration statuses<a name="sns-10dlc-registration-status"></a>

When you register your company or brand, one of two statuses is returned: either **Unverified** or **Verified**\. If the status for your company registration is **Unverified**, it means that there was an issue with your registration\. For example, the Registered Company Name that you provided might not exactly match the registered name of the company associated with the Tax ID that you provided\. If you find a problem with your company registration details, you can correct them\. For more information about modifying your company registration details, see [Editing or deleting a registered company](sns-settings-10dlc-modify-company.md)\.

If the status for your company registration is Verified, then the registration details you provided were accurate, and you can begin creating 10DLC campaigns\.

## Registering your company or brand<a name="sns-register-company-steps-10dlc"></a>

You need to register your company only once\. After it's registered, you can edit your company and contact information\. To delete a registered company, create a case with [AWS Support](https://console.aws.amazon.com/support/home#/)\. For more information about editing or deleting company details, see [Editing or deleting a registered company](sns-settings-10dlc-modify-company.md)\.

**To register a company**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, choose **Register company**\.
**Note**  
The **Register your company** page displays the **Registration fee**\. This is a one\-time fee associated with registering your company\. This cost is separate from any other monthly costs or fees\. It is charged to you when you register your company, or when you modify the details of an existing company registration\.

1. In the **Company info** section, do the following:
   + For **Legal company name**, enter the name that the company is registered under\. The name that you enter must be an exact match for the company name that's associated with the tax ID that you provide\.
**Important**  
Make sure to use your company's exact legal name\. Once submitted you can't change this information\. Incorrect or incomplete information might result in your registration being delayed or denied\.
   + For **What type of legal form is this organization**, choose the option that best describes your company\.
**Note**  
The **US government** and **Not\-for\-profit** options can only be used to register United States\-based organizations\. If your organization is based in a country other than the US, you must register as **Private for\-profit**, regardless of the actual legal form of your organization\.
   + If you chose **Public for profit** in the previous step, enter the company's stock symbol and the stock exchange that it's listed on\.
   + For **Country of registration**, choose the country where the company is registered\.
   + For **Doing Business As \(DBA\) or brand name**, enter any other names that your company does business as\.
   + For **Tax ID**, enter your company's tax ID\. The ID that you enter depends on the country that your company is registered in\.
     + If you're registering a US or non\-US entity that has an IRS Employer Identification Number \(EIN\), enter your nine\-digit EIN\. The legal company name, EIN, and physical address that you enter must all match the company information that is registered with the IRS\.
     + If you're registering a Canadian entity, enter your federal or provincial Corporation number\. Don't enter the Business Number \(BN\) provided by the CRA\. The legal company name, Corporation number, and physical address that you enter must all match the company information that is registered with Corporations Canada\.
     + If you're registering an entity that is based in another country, enter the primary tax ID for your country\. In many countries, this is the numeric portion of your VAT ID number\.
   + For **Vertical**, choose the category that best describes the company you're registering\.

1. In the **Contact info** section, do the following:
   + For **Address/Street**, enter the physical street address associated with your company\.
   + For **City**, enter the city where the physical address is located\.
   + For **State or region**, enter the state or region where the address is located\.
   + For **Zip Code/Postal Code**, enter the ZIP or postal code for the address\.
   + For **Company website**, enter the full URL of your company's website\. Include "http://" or "https://" at the beginning of the address\.
   + For **Support email**, enter an email address\.
   + For **Support phone number**, enter a phone number with a country code\.
**Note**  
The Campaign Registry requires a contact email address and phone number in case they need to verify the registration information with a representative of your company\.

1. When you finish, choose **Create**\. Your company registration is submitted to the Campaign Registry\. In most cases, your registration is accepted immediately, and a status is provided\.

If the status for your company registration is **Verified**, then you can begin creating low\-volume, mixed\-use 10DLC campaigns\. You can use this type of campaign to send up to 75 messages per minute to recipients who use AT&T, and your registered company can send 2,000 messages per day to recipients who use T\-Mobile\. You can also send messages to recipients who use other US carriers, such as Verizon and US Cellular\. These carriers don't strictly enforce throughput limits, but they do heavily monitor 10DLC messages for signs of spam and abuse\.

If your use case requires a throughput rate that exceeds these values, you can apply for additional vetting of your company registration\. For more information about vetting your brand registration, see [Vetting your Amazon SNS 10DLC registration](#sns-10dlc-vetting)\.

If the status for your company registration is **Unverified**, there were issues with the information that you provided\. Check the information that you provided and confirm that all of the fields contain the correct information\. You can make changes to some parts of your company registration in the Amazon Pinpoint console\. For more information about modifying your company registration details, see [Editing a 10DLC company registration](sns-settings-10dlc-modify-company.md#sns-edit-company-10dlc)\.

## Vetting your Amazon SNS 10DLC registration<a name="sns-10dlc-vetting"></a>

If your company registration is successful and you want to register a campaign with higher throughput capabilities, then you must vet your company registration\.

When you vet your registration, a third\-party organization analyzes the company details that you provided and returns a vetting score\. A high vetting score can lead to higher throughput rates for your 10DLC company and the campaigns associated with it\. However, vetting isn't guaranteed to increase your throughput\. 

Vetting scores aren't applied retroactively\. In other words, if you've already created a 10DLC campaign, and you later vet your company registration, your vetting score isn't automatically applied to your existing campaign\. For this reason, you should vet your company or brand before you create any of your 10DLC campaigns\. 

**Note**  
There is a $40 non\-refundable fee for vetting your company or brand\.

**To vet your company registration**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, choose the **10DLC company** that you want to vet\.

1. On the company details page, toward the bottom of the page, choose **Apply for vetting**\.

1. On the **Apply for additional vetting** window, choose **Submit**\.

   For US\-based companies, the vetting process typically takes around a minute to complete\. For non\-US\-based companies, the vetting process might take significantly more time, depending on how readily available data is for that country\. 

After you submit a vetting request, you return to the company details page\. The **Company vetting results **section displays the status and results of your vetting request\. When the vetting process completes, this table shows a vetting score in the **Score** column\. Your vetting score determines your 10DLC throughput capabilities\. Your throughput varies based on the type of campaign that you create\. If you create mixed\-use or marketing\-related 10DLC campaigns, you need to have a higher vetting score than you'd need for other campaign types in order to achieve high throughput rates\. For more information about the capabilities of 10DLC phone numbers, see [10DLC capabilities](channels-sms-originating-identities-10dlc.md#sns-10dlc-capabilities)\. 

If you change the details of your company registration after completing the vetting process, you can request to have your registration vetted again\. If you only change the Vertical for your company registration, your vetting score won't change\. If you change any details other than the Vertical, your vetting result could change\. In either case, you're charged the one\-time vetting fee again\. 