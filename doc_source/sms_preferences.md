# Setting SMS messaging preferences<a name="sms_preferences"></a>

Use Amazon SNS to specify preferences for SMS messaging\. For example, you can specify whether to optimize deliveries for cost or reliability, your monthly spending limit, how deliveries are logged, and whether to subscribe to daily SMS usage reports\.

These preferences take effect for every SMS message that you send from your account, but you can override some of them when you send an individual message\. For more information, see [Publishing to a mobile phone](sms_publish-to-phone.md)\.

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

To set your SMS preferences using one of the AWS SDKs, use the action in that SDK that corresponds to the `SetSMSAttributes` request in the Amazon SNS API\. With this request, you assign values to the different SMS attributes, such as your monthly spend quota and your default SMS type \(promotional or transactional\)\. For all SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the *Amazon Simple Notification Service API Reference*\.

The following code examples show how to set the default settings for sending SMS messages using Amazon SNS\.

------
#### [ C\+\+ ]

**SDK for C\+\+**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
How to use Amazon SNS to set the DefaultSMSType attribute\.  

```
  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::SNS::SNSClient sns;
    Aws::String sms_type =  argv[1];

    Aws::SNS::Model::SetSMSAttributesRequest ssmst_req;
    ssmst_req.AddAttributes("DefaultSMSType", sms_type);

    auto ssmst_out = sns.SetSMSAttributes(ssmst_req);

    if (ssmst_out.IsSuccess())
    {
      std::cout << "SMS Type set successfully " << std::endl;
    }
    else
    {
        std::cout << "Error while setting SMS Type: '" << ssmst_out.GetError().GetMessage()
            << "'" << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
```
+  For API details, see [SetSmsAttributes](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/SetSmsAttributes) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
public class SetSMSAttributes {
    public static void main(String[] args) {

        HashMap<String, String> attributes = new HashMap<>(1);
        attributes.put("DefaultSMSType", "Transactional");
        attributes.put("UsageReportS3Bucket", "janbucket" );

        SnsClient snsClient = SnsClient.builder()
            .region(Region.US_EAST_1)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();
        setSNSAttributes(snsClient, attributes);
        snsClient.close();
    }

    public static void setSNSAttributes( SnsClient snsClient, HashMap<String, String> attributes) {

        try {
            SetSmsAttributesRequest request = SetSmsAttributesRequest.builder()
                .attributes(attributes)
                .build();

            SetSmsAttributesResponse result = snsClient.setSMSAttributes(request);
            System.out.println("Set default Attributes to " + attributes + ". Status was " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [SetSmsAttributes](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/SetSmsAttributes) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sns#code-examples)\. 
Create the client in a separate module and export it\.  

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js
import {SetSMSAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  attributes: {
    /* required */
    DefaultSMSType: "Transactional" /* highest reliability */,
    //'DefaultSMSType': 'Promotional' /* lowest cost */
  },
};

const run = async () => {
  try {
    const data = await snsClient.send(new SetSMSAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-sending-sms.html#sending-sms-setattributes)\. 
+  For API details, see [SetSmsAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/setsmsattributescommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

try {
    $result = $SnSclient->SetSMSAttributes([
        'attributes' => [
            'DefaultSMSType' => 'Transactional',
        ],
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-sending-sms.html#set-sms-attributes)\. 
+  For API details, see [SetSmsAttributes](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/SetSmsAttributes) in *AWS SDK for PHP API Reference*\. 

------