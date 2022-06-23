# SMS requirements for Singapore<a name="channels-sms-singapore-requirements"></a>

Amazon SNS customers are able to send SMS traffic in Singapore using a *Sender ID* that has been registered through the Singapore SMS Sender ID Registry \(SSIR\)\. SSIR was launched in March 2022 through the Singapore Network Information Centre \(SGNIC\) which is owned by Info\-communications Media Development Authority \(IMDA\) of Singapore, and enables organizations to register their Sender ID when sending SMS to mobile phones in Singapore\.

Before you can begin using a Singapore Sender ID registered through SSIR, you must complete an onboarding process through Amazon to allow\-list your account for usage of your registered Sender ID prior to initiating the registration through SGNIC\. Instructions for onboarding your registered Sender ID to Amazon are listed below\. If you do not want to use a Singapore registered Sender ID there is no action for you to take, and you can continue sending your messages through Amazon SNS\.

**Important**  
**To register a Sender ID with Singapore Network Information Centre \(SGNIC\), there are two steps that must be completed in the following order\.**   
You must first work with Amazon to register your Singapore \(SG\) Sender ID for your account\. Once this step is complete you can proceed to the next step\.
Work with SGNIC to register your Sender ID\.
Doing these steps out of order may result in your Sender ID being blocked by the service, or prevent your Sender ID from being preserved on the mobile device\.

**Note**  
You are required to submit a Sender ID registration from each individual AWS account you require to use the Sender ID\.

## To register your Singapore Sender ID with Amazon SNS<a name="channels-sms-singapore-requirements-sender-id"></a>

1. Complete the steps at [Requesting sender IDs for SMS messaging with Amazon SNS](channels-sms-awssupport-sender-id.md)\. In your request, provide the following required information\.
   + The AWS Region that you use with Amazon SNS\.
   + The company name\.
   + An estimate of the number of messages that you plan to send each month\.
   + A description of your use case\.
   + Information about the steps that your recipients must complete to opt\-in to receiving your messages\.
   + Confirmation that you collect and manage opt\-ins and opt\-outs\.

1. Complete the form provided to you by the Amazon SNS team after completing the previous step\.

1. Reply back to the case and attach the completed form for processing\.

1. Amazon will inform you in the support case when your Sender ID is successfully registered to your account, and provide you with the necessary routing information to initiate the registration through SGNIC\.