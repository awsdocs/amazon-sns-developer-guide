# Getting information about 10DLC registration issues<a name="sns-settings-rejected-10dlc"></a>

In some situations, you could receive an error message when you attempt to register your company or your 10DLC campaign\.

## Company registration issues<a name="sns-company-registration-issues-10dlc"></a>

When you register your company, you see one of two registration statuses: **Verified** or **Unverified**\. If the company registration status is **Verified**, then your company registration was successful\. You can begin to create 10DLC campaigns\.

If the status for your company registration is **Unverified**, there were issues with the information that you provided\. The Amazon Pinpoint console provides information about the reasons that your company registration received this status\.

**To view registration issues for your 10DLC company registration**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, in the list of campaigns, choose the name of the company that you want to find more information about\.

1. The company detail page contains information about the issues that were identified in your registration\. If a field in the **Company info** section contains a warning symbol, the registration issue is related to the information in that field\. 

   Check the information that you provided and confirm that all of the fields contain the correct information\. You can edit your company registration in the Amazon Pinpoint console\. For more information about modifying your company registration details, see [Editing or deleting a registered company](sns-settings-10dlc-modify-company.md)\.

If you're unable to identify the issue with your registration, you can create a case with AWS Support to request additional information\. Follow the procedure in Campaign registration issues to create a case\.

### Campaign registration issues<a name="sns-campaign-registration-issues-10dlc"></a>

When you register your 10DLC campaign, you might see an error message in certain situations\.

**To submit a request for information about a rejected 10DLC campaign**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Choose **Support**, and then **Support Center**\.

1. On the Support page, choose **Create case**\.

1. For **Case type**, choose **Service limit increase**\.

1. For **Limit type**, choose **Pinpoint SMS**\.

1. In the **Requests** section, do the following:
   + For **Region**, choose the AWS Region that you attempted to register the campaign in\.
   + For **Resource Type**, choose **10DLC Registration**\.
   + For the **Limit**, choose **Company or 10DLC Campaign Registration Rejection**\.

1. For **Use case description**, enter the rejected 10DLC campaign ID\.

1. Under **Contact options**, for **Preferred contact language**, choose the language that you prefer to use when communicating with the AWS Support team\.

1. For **Contact method**, choose your preferred method of communicating with the AWS Support team\.

1. Choose **Submit**\.

The AWS Support team will provide information about the reasons that your 10DLC campaign registration was rejected in your AWS Support case\.