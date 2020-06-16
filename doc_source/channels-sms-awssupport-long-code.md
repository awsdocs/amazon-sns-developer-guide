# Requesting dedicated long codes for SMS messaging with Amazon SNS<a name="channels-sms-awssupport-long-code"></a>

A long code \(also referred to as a long virtual number, or LVN\) is a standard phone number that contains up to 12 digits, depending on the country that it's based in\. Long codes are typically meant for low\-volume, person\-to\-person communication\. However, you can also use long codes for sending test messages, or for sending low volumes of messages to your customers\.

**Note**  
Support for long codes is restricted to the United States only\. Sending rates for long codes are restricted to 1 message per second\. This restriction is set by the telecom carriers, and isn't a limitation of Amazon SNS\. If you send a large volume of messages from a long code, wireless carriers might begin to block your messages\. Your applications that use Amazon SNS should limit the number of messages that they send each second\. 

**Note**  
If you're new to SMS messaging with Amazon SNS, you should also request a monthly SMS spending threshold that meets the expected demands of your SMS use case\. By default, your monthly spending threshold is $1\.00 \(USD\)\. For more information, see [Requesting increases to your monthly SMS spending quota for Amazon SNS](channels-sms-awssupport-spend-threshold.md)\. 

## Step 1: Request a long code using the Amazon Pinpoint console<a name="channels-sms-awssupport-long-code-open"></a>

Request a long code by completing the following steps\.

**To request a dedicated long code**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Choose **Create a project**

1. Select the project you created\.

1. Under **Settings**, choose **SMS and voice**\.

1. Under the **Number settings** heading, choose **Request long codes**\.

1. Under the **Long code specifications** heading, choose the **Target country or region** from the drop\-down\.

1. Under **Default call type** choose **Transactional** or **Promotional**\.

1. Under **Quantity** choose **1**\. Amazon SNS currently only supports a single long code per AWS account\.

1. Optional: If you would like to create long codes in more countries or regions, choose **Add a country or region**\. Amazon SNS currently only supports a single long code per AWS account\.

1. Choose **Request long codes**\.

1. Once your long code is provisioned, you will see it listed under **Number settings \(N\)**\.

1. Under **Number settings**, choose the long code\.

1. Under **Default keywords**, you can optionally edit the Amazon SNS responses for the *HELP* and *STOP* keywords\.

1. Under **Registered keyword**, you can enter a keyword that you registered with the wireless carriers\. Amazon SNS will automatically reply with the **Response message** you enter\.

1. When you finish, choose **Save**\.

## Next steps<a name="channels-sms-awssupport-long-code-next"></a>

You've registered a long code\. Now you can use Amazon SNS to send SMS messages with your long code as the origination number\.