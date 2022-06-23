# Amazon SNS message publishing<a name="sns-publishing"></a>

After you [create an Amazon SNS topic](sns-create-topic.md) and [subscribe](sns-create-subscribe-endpoint-to-topic.md) an endpoint to it, you can *publish* messages to the topic\. When a message is published, Amazon SNS attempts to deliver the message to the subscribed [endpoints](sns-create-subscribe-endpoint-to-topic.md#sns-endpoints)\. 

**Important**  
You can publish messages only to topics and endpoints in the same AWS Region\.

**Topics**
+ [To publish messages to Amazon SNS topics using the AWS Management Console](#sns-publishing-messages)
+ [To publish a message to a topic using an AWS SDK](#publish-to-topic-aws-sdks)
+ [Publishing large messages with Amazon SNS and Amazon S3](large-message-payloads.md)
+ [Amazon SNS message attributes](sns-message-attributes.md)
+ [Amazon SNS message batching](sns-batch-api-actions.md)

## To publish messages to Amazon SNS topics using the AWS Management Console<a name="sns-publishing-messages"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Topics**\.

1. On the **Topics** page, select a topic, and then choose **Publish message**\.

   The console opens the **Publish message to topic** page\.

1. In the **Message details** section, do the following:

   1. \(Optional\) Enter a message **Subject**\.

   1. For a [FIFO topic](sns-fifo-topics.md), enter a **Message group ID**\. Messages in the same message group are delivered in the order that they are published\.

   1. For a FIFO topic, enter a **Message deduplication ID**\. This ID is optional if you enabled the **Content\-based message deduplication** setting for the topic\.

   1. \(Optional\) For [mobile push notifications](sns-ttl.md), enter a **Time to Live \(TTL\)** value in seconds\. This is the amount of time that a push notification service—such as Apple Push Notification Service \(APNs\) or Firebase Cloud Messaging \(FCM\)—has to deliver the message to the endpoint\.

1. In the **Message body** section, do one of the following:

   1. Choose **Identical payload for all delivery protocols**, and then enter a message\.

   1. Choose **Custom payload for each delivery protocol**, and then enter a JSON object to define the message to send for each delivery protocol\.

      For more information, see [Publishing with platform\-specific payload](sns-send-custom-platform-specific-payloads-mobile-devices.md)\.

1. In the **Message attributes** section, add any attributes that you want Amazon SNS to match with the subscription attribute `FilterPolicy` to decide whether the subscribed endpoint is interested in the published message\.

   1. For **Type**, choose an attribute type, such as **String\.Array**\.
**Note**  
For attribute type **String\.Array**, enclose the array in square brackets \(`[]`\)\. Within the array, enclose string values in double quotation marks\. You don't need quotation marks for numbers or for the keywords `true`, `false`, and `null`\.

   1. Enter an attribute **Name**, such as `customer_interests`\.

   1. Enter an attribute **Value**, such as `["soccer", "rugby", "hockey"]`\.

   If the attribute type is **String**, **String\.Array**, or **Number**, Amazon SNS evaluates the message attribute against a subscription's [filter policy](sns-message-filtering.md) \(if present\) before sending the message to the subscription\.

   For more information, see [Amazon SNS message attributes](sns-message-attributes.md)\.

1. Choose **Publish message**\.

   The message is published to the topic, and the console opens the topic's **Details** page\.

## To publish a message to a topic using an AWS SDK<a name="publish-to-topic-aws-sdks"></a>

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

The following code examples show how to publish messages to an Amazon SNS topic\.

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/SNS/SNSMessageExample/#code-examples)\. 
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/DotNetSDKV3/sns-2010-03-31/Publish) in *AWS SDK for \.NET API Reference*\. 

------
#### [ C\+\+ ]

**SDK for C\+\+**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
  

```
  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::SNS::SNSClient sns;
    Aws::String message = argv[1];
    Aws::String topic_arn = argv[2];

    Aws::SNS::Model::PublishRequest psms_req;
    psms_req.SetMessage(message);
    psms_req.SetTopicArn(topic_arn);

    auto psms_out = sns.Publish(psms_req);

    if (psms_out.IsSuccess())
    {
      std::cout << "Message published successfully " << std::endl;
    }
    else
    {
      std::cout << "Error while publishing message " << psms_out.GetError().GetMessage()
        << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
```
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/Publish) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Go ]

**SDK for Go V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/gov2/sns/Publish#code-examples)\. 
+  For API details, see [Publish](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2/service/sns#Client.Publish) in *AWS SDK for Go API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static void pubTopic(SnsClient snsClient, String message, String topicArn) {

        try {
            PublishRequest request = PublishRequest.builder()
                .message(message)
                .topicArn(topicArn)
                .build();

            PublishResponse result = snsClient.publish(request);
            System.out.println(result.messageId() + " Message sent. Status is " + result.sdkHttpResponse().statusCode());

         } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
         }
    }
```
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/Publish) in *AWS SDK for Java 2\.x API Reference*\. 

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
import {PublishCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
var params = {
  Message: "MESSAGE_TEXT", // MESSAGE_TEXT
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
};

const run = async () => {
  try {
    const data = await snsClient.send(new PublishCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-publishing-messages.html)\. 
+  For API details, see [Publish](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/publishcommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/secretsmanager#code-examples)\. 
  

```
suspend fun pubTopic(topicArnVal: String, messageVal: String) {

    val request = PublishRequest{
        message = messageVal
        topicArn = topicArnVal
     }

    SnsClient { region = "us-east-1" }.use { snsClient ->
        val result = snsClient.publish(request)
        println("${result.messageId} message sent.")
    }
}
```
+  For API details, see [Publish](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Sends a message to an Amazon SNS topic.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$message = 'This message is sent from a Amazon SNS code sample.';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->publish([
        'Message' => $message,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-subscribing-unsubscribing-topics.html#publish-a-message-to-an-sns-topic)\. 
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/Publish) in *AWS SDK for PHP API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/sns#code-examples)\. 
Publish a message with attributes so that a subscription can filter based on attributes\.  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def publish_message(topic, message, attributes):
        """
        Publishes a message, with attributes, to a topic. Subscriptions can be filtered
        based on message attributes so that a subscription receives messages only
        when specified attributes are present.

        :param topic: The topic to publish to.
        :param message: The message to publish.
        :param attributes: The key-value attributes to attach to the message. Values
                           must be either `str` or `bytes`.
        :return: The ID of the message.
        """
        try:
            att_dict = {}
            for key, value in attributes.items():
                if isinstance(value, str):
                    att_dict[key] = {'DataType': 'String', 'StringValue': value}
                elif isinstance(value, bytes):
                    att_dict[key] = {'DataType': 'Binary', 'BinaryValue': value}
            response = topic.publish(Message=message, MessageAttributes=att_dict)
            message_id = response['MessageId']
            logger.info(
                "Published message with attributes %s to topic %s.", attributes,
                topic.arn)
        except ClientError:
            logger.exception("Couldn't publish message to topic %s.", topic.arn)
            raise
        else:
            return message_id
```
Publish a message that takes different forms based on the protocol of the subscriber\.  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def publish_multi_message(
            topic, subject, default_message, sms_message, email_message):
        """
        Publishes a multi-format message to a topic. A multi-format message takes
        different forms based on the protocol of the subscriber. For example,
        an SMS subscriber might receive a short, text-only version of the message
        while an email subscriber could receive an HTML version of the message.

        :param topic: The topic to publish to.
        :param subject: The subject of the message.
        :param default_message: The default version of the message. This version is
                                sent to subscribers that have protocols that are not
                                otherwise specified in the structured message.
        :param sms_message: The version of the message sent to SMS subscribers.
        :param email_message: The version of the message sent to email subscribers.
        :return: The ID of the message.
        """
        try:
            message = {
                'default': default_message,
                'sms': sms_message,
                'email': email_message
            }
            response = topic.publish(
                Message=json.dumps(message), Subject=subject, MessageStructure='json')
            message_id = response['MessageId']
            logger.info("Published multi-format message to topic %s.", topic.arn)
        except ClientError:
            logger.exception("Couldn't publish message to topic %s.", topic.arn)
            raise
        else:
            return message_id
```
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/Publish) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------
#### [ Ruby ]

**SDK for Ruby**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/ruby/example_code/sns#code-examples)\. 
  

```
require 'aws-sdk-sns'  # v2: require 'aws-sdk'

def message_sent?(sns_client, topic_arn, message)

  sns_client.publish(topic_arn: topic_arn, message: message)
rescue StandardError => e
  puts "Error while sending the message: #{e.message}"
  end

def run_me

  topic_arn = 'SNS_TOPIC_ARN'
  region = 'REGION'
  message = 'MESSAGE' # The text of the message to send.

  sns_client = Aws::SNS::Client.new(region: region)

  puts "Message sending."

  if message_sent?(sns_client, topic_arn, message)
    puts 'The message was sent.'
  else
    puts 'The message was not sent. Stopping program.'
    exit 1
  end
  end

run_me if $PROGRAM_NAME == __FILE__
```
+  For more information, see [AWS SDK for Ruby Developer Guide](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/sns-example-send-message.html)\. 
+  For API details, see [Publish](https://docs.aws.amazon.com/goto/SdkForRubyV3/sns-2010-03-31/Publish) in *AWS SDK for Ruby API Reference*\. 

------
#### [ Rust ]

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```
async fn subscribe_and_publish(
    client: &Client,
    topic_arn: &str,
    email_address: &str,
) -> Result<(), Error> {
    println!("Receiving on topic with ARN: `{}`", topic_arn);

    let rsp = client
        .subscribe()
        .topic_arn(topic_arn)
        .protocol("email")
        .endpoint(email_address)
        .send()
        .await?;

    println!("Added a subscription: {:?}", rsp);

    let rsp = client
        .publish()
        .topic_arn(topic_arn)
        .message("hello sns!")
        .send()
        .await?;

    println!("Published message: {:?}", rsp);

    Ok(())
}
```
+  For API details, see [Publish](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

------