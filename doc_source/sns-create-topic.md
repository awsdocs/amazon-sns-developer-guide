# Creating an Amazon SNS topic<a name="sns-create-topic"></a>

An Amazon SNS topic is a logical access point that acts as a *communication channel*\. A topic lets you group multiple *endpoints* \(such as AWS Lambda, Amazon SQS, HTTP/S, or an email address\)\.

To broadcast the messages of a message\-producer system \(for example, an e\-commerce website\) working with multiple other services that require its messages \(for example, checkout and fulfillment systems\), you can create a topic for your producer system\.

The first and most common Amazon SNS task is creating a topic\. This page shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to create a topic\.

During creation, you choose a topic type \(standard or FIFO\) and name the topic\. After creating a topic, you can't change the topic type or name\. All other configuration choices are optional during topic creation, and you can edit them later\.

**Important**  
Do not add personally identifiable information \(PII\) or other confidential or sensitive information in topic names\. Topic names are accessible to other Amazon Web Services, including CloudWatch Logs\. Topic names are not intended to be used for private or sensitive data\.

**Topics**
+ [AWS Management Console](#create-topic-aws-console)
+ [AWS SDKs](#create-topic-aws-sdks)

## To create a topic using the AWS Management Console<a name="create-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Do one of the following:
   + If no topics have ever been created under your AWS account before, read the description of Amazon SNS on the home page\.
   + If topics have been created under your AWS account before, on the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose **Create topic**\.

1. On the **Create topic** page, in the **Details** section, do the following:

   1. For **Type**, choose a topic type \(**Standard** or **FIFO**\)\.

   1. Enter a **Name** for the topic\. For a [FIFO topic](sns-fifo-topics.md), add **\.fifo** to the end of the name\.

   1. \(Optional\) Enter a **Display name** for the topic\.

   1. \(Optional\) For a FIFO topic, you can choose **content\-based message deduplication** to enable default message deduplication\. For more information, see [Message deduplication for FIFO topics](fifo-message-dedup.md)\.

1. \(Optional\) Expand the **Encryption** section and do the following\. For more information, see [Encryption at rest](sns-server-side-encryption.md)\.

   1. Choose **Enable encryption**\.

   1. Specify the AWS KMS key\. For more information, see [Key terms](sns-server-side-encryption.md#sse-key-terms)\.

      For each KMS type, the **Description**, **Account**, and **KMS ARN** are displayed\.
**Important**  
If you aren't the owner of the KMS, or if you log in with an account that doesn't have the `kms:ListAliases` and `kms:DescribeKey` permissions, you won't be able to view information about the KMS on the Amazon SNS console\.  
Ask the owner of the KMS to grant you these permissions\. For more information, see the [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.
      + The AWS managed KMS for Amazon SNS **\(Default\) alias/aws/sns** is selected by default\.
**Note**  
Keep the following in mind:  
The first time you use the AWS Management Console to specify the AWS managed KMS for Amazon SNS for a topic, AWS KMS creates the AWS managed KMS for Amazon SNS\.
Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed KMS for Amazon SNS\.
      + To use a custom KMS from your AWS account, choose the **AWS KMS key** field and then choose the custom KMS from the list\.
**Note**  
For instructions on creating custom KMSs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*
      + To use a custom KMS ARN from your AWS account or from another AWS account, enter it into the **AWS KMS key** field\.

1. \(Optional\) By default, only the topic owner can publish or subscribe to the topic\. To configure additional access permissions, expand the **Access policy** section\. For more information, see [Identity and access management in Amazon SNS](sns-authentication-and-access-control.md) and [Example cases for Amazon SNS access control](sns-access-policy-use-cases.md)\. 
**Note**  
When you create a topic using the console, the default policy uses the `aws:SourceOwner` condition key\. This key is similar to `aws:SourceAccount`\. 

1. \(Optional\) To configure how Amazon SNS retries failed message delivery attempts, expand the **Delivery retry policy \(HTTP/S\)** section\. For more information, see [Amazon SNS message delivery retries](sns-message-delivery-retries.md)\.

1. \(Optional\) To configure how Amazon SNS logs the delivery of messages to CloudWatch, expand the **Delivery status logging** section\. For more information, see [Amazon SNS message delivery status](sns-topic-attributes.md)\.

1. \(Optional\) To add metadata tags to the topic, expand the **Tags** section, enter a **Key** and a **Value** \(optional\) and choose **Add tag**\. For more information, see [Amazon SNS topic tagging](sns-tags.md)\.

1. Choose **Create topic**\.

   The topic is created and the ***MyTopic*** page is displayed\.

   The topic's **Name**, **ARN**, \(optional\) **Display name**, and **Topic owner**'s AWS account ID are displayed in the **Details** section\.

1. Copy the topic ARN to the clipboard, for example:

   ```
   arn:aws:sns:us-east-2:123456789012:MyTopic
   ```

## To create a topic using an AWS SDK<a name="create-topic-aws-sdks"></a>

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

The following code examples show how to create an Amazon SNS topic\.

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
    /// This example shows how to use Amazon Simple Notification Service
    /// (Amazon SNS) to add a new Amazon SNS topic. The example was created
    /// using the AWS SDK for .NET version 3.7 and .NET Core 5.0.
    /// </summary>
    public class CreateSNSTopic
    {
        public static async Task Main()
        {
            string topicName = "ExampleSNSTopic";

            IAmazonSimpleNotificationService client = new AmazonSimpleNotificationServiceClient();

            var topicArn = await CreateSNSTopicAsync(client, topicName);
            Console.WriteLine($"New topic ARN: {topicArn}");
        }

        /// <summary>
        /// Creates a new SNS topic using the supplied topic name.
        /// </summary>
        /// <param name="client">The initialized SNS client object used to
        /// create the new topic.</param>
        /// <param name="topicName">A string representing the topic name.</param>
        /// <returns>The Amazon Resource Name (ARN) of the created topic.</returns>
        public static async Task<string> CreateSNSTopicAsync(IAmazonSimpleNotificationService client, string topicName)
        {
            var request = new CreateTopicRequest
            {
                Name = topicName,
            };

            var response = await client.CreateTopicAsync(request);

            return response.TopicArn;
        }

    }
```
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/DotNetSDKV3/sns-2010-03-31/CreateTopic) in *AWS SDK for \.NET API Reference*\. 

------
#### [ C\+\+ ]

**SDK for C\+\+**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/cpp/example_code/sns#code-examples)\. 
  

```
  Aws::SDKOptions options;
  Aws::InitAPI(options);
  {
    Aws::String topic_name = argv[1];
    Aws::SNS::SNSClient sns;

    Aws::SNS::Model::CreateTopicRequest ct_req;
    ct_req.SetName(topic_name);

    auto ct_out = sns.CreateTopic(ct_req);

    if (ct_out.IsSuccess())
    {
      std::cout << "Successfully created topic " << topic_name << std::endl;
    }
    else
    {
      std::cout << "Error creating topic " << topic_name << ":" <<
        ct_out.GetError().GetMessage() << std::endl;
    }
  }

  Aws::ShutdownAPI(options);
```
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/SdkForCpp/sns-2010-03-31/CreateTopic) in *AWS SDK for C\+\+ API Reference*\. 

------
#### [ Go ]

**SDK for Go V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/gov2/sns/CreateTopic#code-examples)\. 
+  For API details, see [CreateTopic](https://pkg.go.dev/github.com/aws/aws-sdk-go-v2/service/sns#Client.CreateTopic) in *AWS SDK for Go API Reference*\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
    public static String createSNSTopic(SnsClient snsClient, String topicName ) {

        CreateTopicResponse result = null;
        try {
            CreateTopicRequest request = CreateTopicRequest.builder()
                .name(topicName)
                .build();

            result = snsClient.createTopic(request);
            return result.topicArn();

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
        return "";
    }
```
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/CreateTopic) in *AWS SDK for Java 2\.x API Reference*\. 

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
import {CreateTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { Name: "TOPIC_NAME" }; //TOPIC_NAME

const run = async () => {
  try {
    const data = await snsClient.send(new CreateTopicCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-managing-topics.html#sns-examples-managing-topics-createtopic)\. 
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/createtopiccommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/sns#code-examples)\. 
  

```
suspend fun createSNSTopic(topicName: String): String {

    val request = CreateTopicRequest {
        name = topicName
    }

    SnsClient { region = "us-east-1" }.use { snsClient ->
        val result = snsClient.createTopic(request)
        return result.topicArn.toString()
    }
}
```
+  For API details, see [CreateTopic](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Create a Simple Notification Service topics in your AWS account at the requested region.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);

$topicname = 'myTopic';

try {
    $result = $SnSclient->createTopic([
        'Name' => $topicname,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  For more information, see [AWS SDK for PHP Developer Guide](https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/sns-examples-managing-topics.html#create-a-topic)\. 
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/CreateTopic) in *AWS SDK for PHP API Reference*\. 

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

    def create_topic(self, name):
        """
        Creates a notification topic.

        :param name: The name of the topic to create.
        :return: The newly created topic.
        """
        try:
            topic = self.sns_resource.create_topic(Name=name)
            logger.info("Created topic %s with ARN %s.", name, topic.arn)
        except ClientError:
            logger.exception("Couldn't create topic %s.", name)
            raise
        else:
            return topic
```
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/CreateTopic) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------
#### [ Ruby ]

**SDK for Ruby**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/ruby/example_code/sns#code-examples)\. 
  

```
require 'aws-sdk-sns'  # v2: require 'aws-sdk'

def topic_created?(sns_client, topic_name)

sns_client.create_topic(name: topic_name)
rescue StandardError => e
  puts "Error while creating the topic named '#{topic_name}': #{e.message}"
end

# Full example call:
def run_me
  topic_name = 'TOPIC_NAME'
  region = 'REGION'

  sns_client = Aws::SNS::Client.new(region: region)

  puts "Creating the topic '#{topic_name}'..."

  if topic_created?(sns_client, topic_name)
    puts 'The topic was created.'
  else
    puts 'The topic was not created. Stopping program.'
    exit 1
  end
end

run_me if $PROGRAM_NAME == __FILE__
```
+  For more information, see [AWS SDK for Ruby Developer Guide](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/sns-example-create-topic.html)\. 
+  For API details, see [CreateTopic](https://docs.aws.amazon.com/goto/SdkForRubyV3/sns-2010-03-31/CreateTopic) in *AWS SDK for Ruby API Reference*\. 

------
#### [ Rust ]

**SDK for Rust**  
This documentation is for an SDK in preview release\. The SDK is subject to change and should not be used in production\.
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/rust_dev_preview/sns#code-examples)\. 
  

```
async fn make_topic(client: &Client, topic_name: &str) -> Result<(), Error> {
    let resp = client.create_topic().name(topic_name).send().await?;

    println!(
        "Created topic with ARN: {}",
        resp.topic_arn().unwrap_or_default()
    );

    Ok(())
}
```
+  For API details, see [CreateTopic](https://docs.rs/releases/search?query=aws-sdk) in *AWS SDK for Rust API reference*\. 

------