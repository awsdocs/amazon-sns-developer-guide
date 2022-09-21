# Managing phone numbers and SMS subscriptions<a name="sms_manage"></a>

Amazon SNS provides several options for managing who receives SMS messages from your account\. With a limited frequency, you can opt in phone numbers that have opted out of receiving SMS messages from your account\. To stop sending messages to SMS subscriptions, you can remove subscriptions or the topics that publish to them\.

**Topics**
+ [Opting out of receiving SMS messages](#sms_manage_optout)
+ [Managing phone numbers and subscriptions \(console\)](#sms_manage_console)
+ [Managing phone numbers and subscriptions \(AWS SDKs\)](#sms_manage_sdk)

## Opting out of receiving SMS messages<a name="sms_manage_optout"></a>

Where required by local laws and regulations \(such as the US and Canada\), SMS recipients can use their devices to opt out by replying to the message with any of the following: 
+ ARRET \(French\)
+ CANCEL
+ END
+ OPT\-OUT
+ OPTOUT
+ QUIT
+ REMOVE
+ STOP
+ TD
+ UNSUBSCRIBE

To opt out, the recipient must reply to the same [origination number](channels-sms-originating-identities-origination-numbers.md) that Amazon SNS used to deliver the message\. After opting out, the recipient will no longer receive SMS messages delivered from your AWS account unless you opt in the phone number\.

If the phone number is subscribed to an Amazon SNS topic, opting out does not remove the subscription, but SMS messages will fail to deliver to that subscription unless you opt in the phone number\.

## Managing phone numbers and subscriptions \(console\)<a name="sms_manage_console"></a>

You can use the Amazon SNS console to control which phone numbers receive SMS messages from your account\.

### Opting in a phone number that has been opted out<a name="sms_manage_optout_console"></a>

You can view which phone numbers have been opted out of receiving SMS messages from your account, and you can opt in these phone numbers to resume sending messages to them\.

You can opt in a phone number only once every 30 days\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, set the region selector to a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Text messaging \(SMS\)** page, choose **View opted out phone numbers**\. The **Opted out phone numbers** page displays the opted out phone numbers\.

1. Select the check box for the phone number that you want to opt in, and choose **Opt in**\. The phone number is no longer opted out and will receive SMS messages that you send to it\.

### Deleting an SMS subscription<a name="sms_manage_subscriptions_console"></a>

Delete an SMS subscription to stop sending SMS messages to that phone number when you publish to your topics\.

1. On the navigation panel, choose **Subscriptions**\.

1. Select the check boxes for the subscriptions that you want to delete\. Then choose **Actions**, and choose **Delete Subscriptions**\.

1. In the **Delete** window, choose **Delete**\. Amazon SNS deletes the subscription and displays a success message\.

### Deleting a topic<a name="sms_manage_topic_console"></a>

Delete a topic when you no longer want to publish messages to its subscribed endpoints\.

1. On the navigation panel, choose **Topics**\.

1. Select the check boxes for the topics that you want to delete\. Then choose **Actions**, and choose **Delete Topics**\.

1. In the **Delete** window, choose **Delete**\. Amazon SNS deletes the topic and displays a success message\.

## Managing phone numbers and subscriptions \(AWS SDKs\)<a name="sms_manage_sdk"></a>

You can use the AWS SDKs to make programmatic requests to Amazon SNS and manage which phone numbers can receive SMS messages from your account\.

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

### Viewing all opted out phone numbers<a name="sms_view_optout_sdk"></a>

To view all opted out phone numbers, submit a `ListPhoneNumbersOptedOut` request with the Amazon SNS API\.

The following code examples show how to list phone numbers that are opted out of receiving Amazon SNS messages\.

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static void listOpts( SnsClient snsClient) {

        try {
            ListPhoneNumbersOptedOutRequest request = ListPhoneNumbersOptedOutRequest.builder().build();
            ListPhoneNumbersOptedOutResponse result = snsClient.listPhoneNumbersOptedOut(request);
            System.out.println("Status is " + result.sdkHttpResponse().statusCode() + "\n\nPhone Numbers: \n\n" + result.phoneNumbers());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [ListPhoneNumbersOptedOut](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/ListPhoneNumbersOptedOut) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Returns a list of phone numbers that are opted out of receiving SMS messages from your AWS SNS account.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

try {
    $result = $SnSclient->listPhoneNumbersOptedOut([
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-sending-sms.html#list-opted-out-phone-numbers)\. 
+  For API details, see [ListPhoneNumbersOptedOut](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/ListPhoneNumbersOptedOut) in *AWS SDK for PHP API Reference*\. 

------

### Checking whether a phone number is opted out<a name="sms_check_optout_sdk"></a>

To check whether a phone number is opted out, submit a `CheckIfPhoneNumberIsOptedOut` request with the Amazon SNS API\.

The following code examples show how to check whether a phone number is opted out of receiving Amazon SNS messages\.

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/SNS#code-examples)\. 
  

```
    using System;
    using System.Threading.Tasks;
    using Amazon.SimpleNotificationService;
    using Amazon.SimpleNotificationService.Model;

    /// <summary>
    /// This example shows how to use the Amazon Simple Notification Service
    /// (Amazon SNS) to check whether a phone number has been opted out. The
    /// example was created using the AWS SDK for .NET version 3.7 and
    /// .NET Core 5.0.
    /// </summary>
    public class IsPhoneNumOptedOut
    {
        public static async Task Main()
        {
            string phoneNumber = "+15551112222";

            IAmazonSimpleNotificationService client = new AmazonSimpleNotificationServiceClient();

            await CheckIfOptedOutAsync(client, phoneNumber);
        }

        /// <summary>
        /// Checks to see if the supplied phone number has been opted out.
        /// </summary>
        /// <param name="client">The initialized Amazon SNS Client object used
        /// to check if the phone number has been opted out.</param>
        /// <param name="phoneNumber">A string representing the phone number
        /// to check.</param>
        public static async Task CheckIfOptedOutAsync(IAmazonSimpleNotificationService client, string phoneNumber)
        {
            var request = new CheckIfPhoneNumberIsOptedOutRequest
            {
                PhoneNumber = phoneNumber,
            };

            try
            {
                var response = await client.CheckIfPhoneNumberIsOptedOutAsync(request);

                if (response.HttpStatusCode == System.Net.HttpStatusCode.OK)
                {
                    string optOutStatus = response.IsOptedOut ? "opted out" : "not opted out.";
                    Console.WriteLine($"The phone number: {phoneNumber} is {optOutStatus}");
                }
            }
            catch (AuthorizationErrorException ex)
            {
                Console.WriteLine($"{ex.Message}");
            }
        }
    }
```
+  For API details, see [CheckIfPhoneNumberIsOptedOut](https://docs.aws.amazon.com/goto/DotNetSDKV3/sns-2010-03-31/CheckIfPhoneNumberIsOptedOut) in *AWS SDK for \.NET API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static void checkPhone(SnsClient snsClient, String phoneNumber) {

        try {
            CheckIfPhoneNumberIsOptedOutRequest request = CheckIfPhoneNumberIsOptedOutRequest.builder()
                .phoneNumber(phoneNumber)
                .build();

            CheckIfPhoneNumberIsOptedOutResponse result = snsClient.checkIfPhoneNumberIsOptedOut(request);
            System.out.println(result.isOptedOut() + "Phone Number " + phoneNumber + " has Opted Out of receiving sns messages." +
                "\n\nStatus was " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [CheckIfPhoneNumberIsOptedOut](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/CheckIfPhoneNumberIsOptedOut) in *AWS SDK for Java 2\.x API Reference*\. 

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
import {CheckIfPhoneNumberIsOptedOutCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { phoneNumber: "353861230764" }; //PHONE_NUMBER, in the E.164 phone number structure

const run = async () => {
  try {
    const data = await snsClient.send(
      new CheckIfPhoneNumberIsOptedOutCommand(params)
    );
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-sending-sms.html#sending-sms-checkifphonenumberisoptedout)\. 
+  For API details, see [CheckIfPhoneNumberIsOptedOut](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/checkifphonenumberisoptedoutcommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Indicates whether the phone number owner has opted out of receiving SMS messages from your AWS SNS account.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$phone = '+1XXX5550100';

try {
    $result = $SnSclient->checkIfPhoneNumberIsOptedOut([
        'phoneNumber' => $phone,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-sending-sms.html#check-if-a-phone-number-has-opted-out)\. 
+  For API details, see [CheckIfPhoneNumberIsOptedOut](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/CheckIfPhoneNumberIsOptedOut) in *AWS SDK for PHP API Reference*\. 

------

### Opting in a phone number that has been opted out<a name="sms_manage_optin_sdk"></a>

To opt in a phone number, submit an `OptInPhoneNumber` request with the Amazon SNS API\.

You can opt in a phone number only once every 30 days\.

### Deleting an SMS subscription<a name="sms_manage_subscriptions_sdk"></a>

To delete an SMS subscription from an Amazon SNS topic, get the subscription ARN by submitting a `ListSubscriptions` request with the Amazon SNS API, and then pass the ARN to an `Unsubscribe` request\.

The following code examples show how to delete an Amazon SNS subscription\.

------
#### [ C\+\+ ]

**SDK for C\+\+**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
  

```
  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::SNS::SNSClient sns;
    Aws::String subscription_arn = argv[1];

    Aws::SNS::Model::UnsubscribeRequest s_req;
    s_req.SetSubscriptionArn(subscription_arn);

    auto s_out = sns.Unsubscribe(s_req);

    if (s_out.IsSuccess())
    {
      std::cout << "Unsubscribed successfully " << std::endl;
    }
    else
    {
      std::cout << "Error while unsubscribing " << s_out.GetError().GetMessage()
        << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
```
+  For API details, see [Unsubscribe](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/Unsubscribe) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static void unSub(SnsClient snsClient, String subscriptionArn) {

        try {
            UnsubscribeRequest request = UnsubscribeRequest.builder()
                .subscriptionArn(subscriptionArn)
                .build();

            UnsubscribeResponse result = snsClient.unsubscribe(request);
            System.out.println("\n\nStatus was " + result.sdkHttpResponse().statusCode()
                + "\n\nSubscription was removed for " + request.subscriptionArn());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [Unsubscribe](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/Unsubscribe) in *AWS SDK for Java 2\.x API Reference*\. 

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
import {UnsubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { SubscriptionArn: "TOPIC_SUBSCRIPTION_ARN" }; //TOPIC_SUBSCRIPTION_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new UnsubscribeCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-managing-topics.html#sns-examples-unsubscribing)\. 
+  For API details, see [Unsubscribe](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/unsubscribecommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/secretsmanager#code-examples)\. 
  

```
suspend fun unSub(subscriptionArnVal: String) {

    val request = UnsubscribeRequest {
        subscriptionArn = subscriptionArnVal
    }

    SnsClient { region = "us-east-1" }.use { snsClient ->
        snsClient.unsubscribe(request)
        println("Subscription was removed for ${request.subscriptionArn}")
    }
}
```
+  For API details, see [Unsubscribe](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Deletes a subscription to an Amazon SNS topic.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$subscription = 'arn:aws:sns:us-east-1:111122223333:MySubscription';

try {
    $result = $SnSclient->unsubscribe([
        'SubscriptionArn' => $subscription,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-subscribing-unsubscribing-topics.html#unsubscribe-from-a-topic)\. 
+  For API details, see [Unsubscribe](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/Unsubscribe) in *AWS SDK for PHP API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/sns#code-examples)\. 
  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def delete_subscription(subscription):
        """
        Unsubscribes and deletes a subscription.
        """
        try:
            subscription.delete()
            logger.info("Deleted subscription %s.", subscription.arn)
        except ClientError:
            logger.exception("Couldn't delete subscription %s.", subscription.arn)
            raise
```
+  For API details, see [Unsubscribe](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/Unsubscribe) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

### Deleting a topic<a name="sms_manage_topic_sdk"></a>

To delete a topic and all of its subscriptions, get the topic ARN by submitting a `ListTopics` request with the Amazon SNS API, and then pass the ARN to the `DeleteTopic` request\.

The following code examples show how to delete an Amazon SNS topic and all subscriptions to that topic\.

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/SNS#code-examples)\. 
  

```
    using System;
    using System.Threading.Tasks;
    using Amazon.SimpleNotificationService;

    /// <summary>
    /// This example deletes an existing Amazon Simple Notification Service
    /// (Amazon SNS) topic. The example was created using the AWS SDK for .NET
    /// version 3.7 and .NET Core 5.0.
    /// </summary>
    public class DeleteSNSTopic
    {
        public static async Task Main()
        {
            string topicArn = "arn:aws:sns:us-east-2:704825161248:ExampleSNSTopic";
            IAmazonSimpleNotificationService client = new AmazonSimpleNotificationServiceClient();

            var response = await client.DeleteTopicAsync(topicArn);
        }
    }
```
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/goto/DotNetSDKV3/sns-2010-03-31/DeleteTopic) in *AWS SDK for \.NET API Reference*\. 

------
#### [ C\+\+ ]

**SDK for C\+\+**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
  

```
  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::String topic_arn = argv[1];
    Aws::SNS::SNSClient sns;

    Aws::SNS::Model::DeleteTopicRequest dt_req;
    dt_req.SetTopicArn(topic_arn);

    auto dt_out = sns.DeleteTopic(dt_req);

    if (dt_out.IsSuccess())
    {
      std::cout << "Successfully deleted topic " << topic_arn << std::endl;
    }
    else
    {
      std::cout << "Error deleting topic " << topic_arn << ":" <<
        dt_out.GetError().GetMessage() << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
```
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/DeleteTopic) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static void deleteSNSTopic(SnsClient snsClient, String topicArn ) {

        try {
            DeleteTopicRequest request = DeleteTopicRequest.builder()
                .topicArn(topicArn)
                .build();

            DeleteTopicResponse result = snsClient.deleteTopic(request);
            System.out.println("\n\nStatus was " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/DeleteTopic) in *AWS SDK for Java 2\.x API Reference*\. 

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
// Load the AWS SDK for Node.js

// Import required AWS SDK clients and commands for Node.js
import {DeleteTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; //TOPIC_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new DeleteTopicCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-managing-topics.html#sns-examples-managing-topics-deletetopic)\. 
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/deletetopiccommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/sns#code-examples)\. 
  

```
suspend fun deleteSNSTopic(topicArnVal: String) {

    val request = DeleteTopicRequest {
        topicArn = topicArnVal
    }

    SnsClient { region = "us-east-1" }.use { snsClient ->
        snsClient.deleteTopic(request)
        println("$topicArnVal was successfully deleted.")
    }
}
```
+  For API details, see [DeleteTopic](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Deletes a SNS topic and all its subscriptions.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->deleteTopic([
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/DeleteTopic) in *AWS SDK for PHP API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/sns#code-examples)\. 
  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def delete_topic(topic):
        """
        Deletes a topic. All subscriptions to the topic are also deleted.
        """
        try:
            topic.delete()
            logger.info("Deleted topic %s.", topic.arn)
        except ClientError:
            logger.exception("Couldn't delete topic %s.", topic.arn)
            raise
```
+  For API details, see [DeleteTopic](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/DeleteTopic) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------