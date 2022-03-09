# Email notifications<a name="sns-email-notifications"></a>

This page describes how to subscribe an [email address](#sns-email-notifications) to an Amazon SNS topic using the AWS Management Console, AWS SDK for Java, or AWS SDK for \.NET\. 

**Notes**  
You can't customize the body of the email message\. The email delivery feature is intended to provide internal system alerts, not marketing messages\.
Email delivery throughput is throttled according to [Amazon SNS quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html#limits_sns)\.

**Important**  
To prevent mailing list recipients from unsubscribing all recipients from Amazon SNS topic emails, see [Set up an email subscription that requires authentication to unsubscribe](http://aws.amazon.com/premiumsupport/knowledge-center/prevent-unsubscribe-all-sns-topic/) from AWS Support\.

## To subscribe an email address to an Amazon SNS topic using the AWS Management Console<a name="create-subscribe-endpoint-to-topic-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, choose the Amazon Resource Name \(ARN\) of a topic\.

   1. For **Protocol**, choose **Email**\.

   1. For **Endpoint**, enter the email address\.

   1. \(Optional\) To configure a filter policy, expand the **Subscription filter policy** section\. For more information, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md)\.

   1. \(Optional\) To configure a dead\-letter queue for the subscription, expand the **Redrive policy \(dead\-letter queue\)** section\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md)\.

   1. Choose **Create subscription**\.

      The console creates the subscription and opens the subscription's **Details** page\.

You must confirm the subscription before the email address can start to receive messages\.

**To confirm a subscription**

1. Check your email inbox and choose **Confirm subscription** in the email from Amazon SNS\.

1. Amazon SNS opens your web browser and displays a subscription confirmation with your subscription ID\.

## To subscribe an email address to an Amazon SNS topic using an AWS SDK<a name="subscribe-email-to-topic-aws-sdks"></a>

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

The following code examples show how to subscribe an email address to an Amazon SNS topic\.

------
#### [ C\+\+ ]

**SDK for C\+\+**  
  

```
/**
 * Subscribe an email address endpoint to a topic - demonstrates how to initiate a subscription to an Amazon SNS topic with delivery
 *  to an email address.
 * 
 * SNS will send a subscription confirmation email to the email address provided which you need to confirm to 
 * receive messages.
 *
 * <protocol_value> set to "email" provides delivery of message via SMTP (see https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html for available protocols).
 * <topic_arn_value> can be obtained from run_list_topics executable and includes the "arn:" prefix.
 */

int main(int argc, char ** argv)
{
  if (argc != 4)
  {
    std::cout << "Usage: subscribe_email <protocol_value=email> <topic_arn_value>"
                 " <email_address>" << std::endl;
    return 1;
  }

  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::SNS::SNSClient sns;
    Aws::String protocol = argv[1];
    Aws::String topic_arn = argv[2];
    Aws::String endpoint = argv[3];

    Aws::SNS::Model::SubscribeRequest s_req;
    s_req.SetTopicArn(topic_arn);
    s_req.SetProtocol(protocol);
    s_req.SetEndpoint(endpoint);

    auto s_out = sns.Subscribe(s_req);

    if (s_out.IsSuccess())
    {
      std::cout << "Subscribed successfully " << std::endl;
    }
    else
    {
      std::cout << "Error while subscribing " << s_out.GetError().GetMessage()
        << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
  return 0;
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/Subscribe) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Go ]

**SDK for Go V2**  
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/gov2/sns/Subscribe#code-examples)\. 
+  For API details, see [Subscribe](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2/service/sns#Client.Subscribe) in *AWS SDK for Go API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
  

```
    public static void subEmail(SnsClient snsClient, String topicArn, String email) {

        try {
            SubscribeRequest request = SubscribeRequest.builder()
                .protocol("email")
                .endpoint(email)
                .returnSubscriptionArn(true)
                .topicArn(topicArn)
                .build();

            SubscribeResponse result = snsClient.subscribe(request);
            System.out.println("Subscription ARN: " + result.subscriptionArn() + "\n\n Status is " + result.sdkHttpResponse().statusCode());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/Subscribe) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ JavaScript ]

**SDK for JavaScript V3**  
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
import {SubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Protocol: "email" /* required */,
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
  Endpoint: "EMAIL_ADDRESS", //EMAIL_ADDRESS
};

const run = async () => {
  try {
    const data = await snsClient.send(new SubscribeCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sns#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-managing-topics.html#sns-examples-subscribing-email)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/subscribecommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun subEmail(topicArnVal: String, email: String) : String {

    val request = SubscribeRequest {
        protocol = "email"
        endpoint = email
        returnSubscriptionArn = true
        topicArn = topicArnVal
    }

    SnsClient { region = "us-east-1" }.use { snsClient ->
        val result = snsClient.subscribe(request)
        return result.subscriptionArn.toString()
    }
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/secretsmanager#code-examples)\. 
+  For API details, see [Subscribe](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Prepares to subscribe an endpoint by sending the endpoint a confirmation message.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$protocol = 'email';
$endpoint = 'sample@example.com';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->subscribe([
        'Protocol' => $protocol,
        'Endpoint' => $endpoint,
        'ReturnSubscriptionArn' => true,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/Subscribe) in *AWS SDK for PHP API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def subscribe(topic, protocol, endpoint):
        """
        Subscribes an endpoint to the topic. Some endpoint types, such as email,
        must be confirmed before their subscriptions are active. When a subscription
        is not confirmed, its Amazon Resource Number (ARN) is set to
        'PendingConfirmation'.

        :param topic: The topic to subscribe to.
        :param protocol: The protocol of the endpoint, such as 'sms' or 'email'.
        :param endpoint: The endpoint that receives messages, such as a phone number
                         (in E.164 format) for SMS messages, or an email address for
                         email messages.
        :return: The newly added subscription.
        """
        try:
            subscription = topic.subscribe(
                Protocol=protocol, Endpoint=endpoint, ReturnSubscriptionArn=True)
            logger.info("Subscribed %s %s to topic %s.", protocol, endpoint, topic.arn)
        except ClientError:
            logger.exception(
                "Couldn't subscribe %s %s to topic %s.", protocol, endpoint, topic.arn)
            raise
        else:
            return subscription
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/sns#code-examples)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/Subscribe) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------
#### [ Ruby ]

**SDK for Ruby**  
  

```
require 'aws-sdk-sns'  # v2: require 'aws-sdk'

def subscription_created?(sns_client, topic_arn, protocol, endpoint)

  sns_client.subscribe(topic_arn: topic_arn, protocol: protocol, endpoint: endpoint)

rescue StandardError => e
  puts "Error while creating the subscription: #{e.message}"
end

# Full example call:
def run_me

protocol = 'email'
endpoint = 'EMAIL_ADDRESS'
topic_arn = 'TOPIC_ARN'
region = 'REGION'

sns_client = Aws::SNS::Client.new(region: region)

puts "Creating the subscription."

  if subscription_created?(sns_client, topic_arn, protocol, endpoint)
    puts 'The subscriptions was created.'
  else
    puts 'The subscription was not created. Stopping program.'
    exit 1
  end
end

run_me if $PROGRAM_NAME == __FILE__
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/ruby/example_code/sns#code-examples)\. 
+  For more information, see [AWS SDK for Ruby Developer Guide](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/sns-example-create-subscription.html)\. 
+  For API details, see [Subscribe](https://docs.aws.amazon.com/goto/SdkForRubyV3/sns-2010-03-31/Subscribe) in *AWS SDK for Ruby API Reference*\. 

------
#### [ Rust ]

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
+  For API details, see [Subscribe](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

------