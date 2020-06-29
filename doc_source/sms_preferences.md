# Setting SMS messaging preferences<a name="sms_preferences"></a>

Use Amazon SNS to specify preferences for SMS messaging, such as how your deliveries are optimized \(for cost or for reliable delivery\), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports\.

These preferences take effect for every SMS message that you send from your account, but you can override some of them when you send an individual message\. For more information, see [Sending an SMS message](sms_publish-to-phone.md)\.

**Topics**
+ [Setting SMS messaging preferences using the AWS Management Console](#sms_preferences_console)
+ [Setting preferences \(AWS SDKs\)](#sms_preferences_sdk)

## Setting SMS messaging preferences using the AWS Management Console<a name="sms_preferences_console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Choose a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Mobile**, **Text messaging \(SMS\)**\.

1. On the **Mobile text messaging \(SMS\)** page, in the **Text messaging preferences** section, choose **Edit**\.

1. On the **Edit text messaging preferences** page, in the **Details** section, do the following:

   1. For **Default message type**, choose one of the following:
      + **Promotional** \(default\) – Non\-critical messages \(for example, marketing\)\. Amazon SNS optimizes message delivery to incur the lowest cost\.
      + **Transactional** – Critical messages that support customer transactions, such as one\-time passcodes for multi\-factor authentication\. Amazon SNS optimizes message delivery to achieve the highest reliability\.

      For pricing information for promotional and transactional messages, see [Global SMS Pricing](https://aws.amazon.com/sns/sms-pricing/)\.

   1. \(Optional\) For **Account spend limit**, enter the amount \(in USD\) that you want to spend on SMS messages each calendar month\.
**Important**  
By default, the spend quota is set to 1\.00 USD\. If you want to raise the service quota, [submit a request](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\.
If the amount set in the console exceeds your service quota, Amazon SNS stops publishing SMS messages\.
Because Amazon SNS is a distributed system, it stops sending SMS messages within minutes of the spend quota being exceeded\. During this interval, if you continue to send SMS messages, you might incur costs that exceed your quota\.

1. \(Optional\) For **Default sender ID**, enter a custom ID, such as your business brand, which is displayed as the sender of the receiving device\.
**Note**  
Support for sender IDs varies by country\.

1. \(Optional\) Enter the name of the **Amazon S3 bucket name for usage reports**\.
**Note**  
The S3 bucket policy must grant write access to Amazon SNS\.

1. Choose **Save changes**\.

## Setting preferences \(AWS SDKs\)<a name="sms_preferences_sdk"></a>

To set your SMS preferences using one of AWS SDKs, use the action in that SDK that corresponds to the `SetSMSAttributes` request in the Amazon SNS API\. With this request, you assign values to the different SMS attributes, such as your monthly spend quota and your default SMS type \(promotional or transactional\)\. For all SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the *Amazon Simple Notification Service API Reference*\.

The following examples show how to set SMS preferences using the Amazon SNS clients that are provided by the AWS SDKs\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [AWS SDK for \.NET Developer Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) or [AWS SDK for Java V2 Developer Guide](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html)

------
#### [ AWS SDK for Java ]

The following example uses the `setSMSAttributes` method of the `AmazonSNSClient` class in the AWS SDK for Java\. This examples sets values for the different attribute names:

```
public static void main(String[] args) {
	AmazonSNSClient snsClient = new AmazonSNSClient();
	setDefaultSmsAttributes(snsClient);
}

public static void setDefaultSmsAttributes(AmazonSNSClient snsClient) {
	SetSMSAttributesRequest setRequest = new SetSMSAttributesRequest()
			.addAttributesEntry("DefaultSenderID", "mySenderID")
			.addAttributesEntry("MonthlySpendLimit", "1")
			.addAttributesEntry("DeliveryStatusIAMRole", 
					"arn:aws:iam::123456789012:role/mySnsRole")
			.addAttributesEntry("DeliveryStatusSuccessSamplingRate", "10")
			.addAttributesEntry("DefaultSMSType", "Transactional")
			.addAttributesEntry("UsageReportS3Bucket", "sns-sms-daily-usage");
	snsClient.setSMSAttributes(setRequest);
	Map<String, String> myAttributes = snsClient.getSMSAttributes(new GetSMSAttributesRequest())
		.getAttributes();
	System.out.println("My SMS attributes:");
	for (String key : myAttributes.keySet()) {
		System.out.println(key + " = " + myAttributes.get(key));
	}
}
```

This example sets the value for the `MonthlySpendLimit` attribute to 1\.00 USD\. By default, this is the maximum amount allowed by Amazon SNS\. If you want to raise the quota, submit [submit a request](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\. For **New limit value**, enter your desired monthly spend quota\. In the **Use Case Description** field, explain that you are requesting an SMS monthly spend quota increase\. The AWS Support team provides an initial response to your request within 24 hours\.

To verify that the attributes were set correctly, the example prints the result of the `getSMSAttributes` method\. When you run this example, the attributes are displayed in the console output window of your IDE:

```
My SMS attributes:
DeliveryStatusSuccessSamplingRate = 10
UsageReportS3Bucket = sns-sms-daily-usage
DefaultSMSType = Transactional
DeliveryStatusIAMRole = arn:aws:iam::123456789012:role/mySnsRole
MonthlySpendLimit = 1
DefaultSenderID = mySenderID
```

------
#### [ AWS SDK for \.NET ]

The following example uses the `SetSMSAttributes` method of the `AmazonSimpleNotificationServiceClient` class in the AWS SDK for \.NET\. This example sets values for the different attribute names:

```
static void Main(string[] args)
{
    AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
    SetDefaultSmsAttributes(snsClient);            
}

public static void SetDefaultSmsAttributes(AmazonSimpleNotificationServiceClient snsClient)
{
    SetSMSAttributesRequest setRequest = new SetSMSAttributesRequest();
    setRequest.Attributes["DefaultSenderID"] = "mySenderID";
    setRequest.Attributes["MonthlySpendLimit"] =  "1";
    setRequest.Attributes["DeliveryStatusIAMRole"] = "arn:aws:iam::123456789012:role/mySnsRole";
    setRequest.Attributes["DeliveryStatusSuccessSamplingRate"] =  "10";
    setRequest.Attributes["DefaultSMSType"] = "Transactional";
    setRequest.Attributes["UsageReportS3Bucket"] = "sns-sms-daily-usage";
    SetSMSAttributesResponse setResponse = snsClient.SetSMSAttributes(setRequest);
    GetSMSAttributesRequest getRequest = new GetSMSAttributesRequest();
    GetSMSAttributesResponse getResponse = snsClient.GetSMSAttributes(getRequest);
    Console.WriteLine("My SMS attributes:");
    foreach (var item in getResponse.Attributes)
    {
        Console.WriteLine(item.Key + " = " + item.Value);
    }
}
```

This example sets the value for the `MonthlySpendLimit` attribute to 1\.00 USD\. By default, this is the maximum amount allowed by Amazon SNS\. If you want to raise the quota, [submit a case](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\. For **New limit value**, enter your desired monthly spend quota\. In the **Use Case Description** field, explain that you are requesting an SMS monthly spend quota increase\. The AWS Support team provides an initial response to your request within 24 hours\.

To verify that the attributes were set correctly, the example prints the result of the `GetSMSAttributes` method\. When you run this example, the attributes are displayed in the console output window of your IDE:

```
My SMS attributes:
DeliveryStatusSuccessSamplingRate = 10
UsageReportS3Bucket = sns-sms-daily-usage
DefaultSMSType = Transactional
DeliveryStatusIAMRole = arn:aws:iam::123456789012:role/mySnsRole
MonthlySpendLimit = 1
DefaultSenderID = mySenderID
```

------