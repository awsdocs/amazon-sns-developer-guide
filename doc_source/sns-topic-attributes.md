# Amazon SNS message delivery status<a name="sns-topic-attributes"></a>

Amazon SNS provides support to log the delivery status of notification messages sent to topics with the following Amazon SNS endpoints: 
+ HTTP
+ Amazon Kinesis Data Firehose
+ AWS Lambda
+ Platform application endpoint
+ Amazon Simple Queue Service

After you configure the message delivery status attributes, log entries are sent to CloudWatch Logs for messages sent to topic subscribers\. Logging message delivery status helps provide better operational insight, such as the following: 
+ Knowing whether a message was delivered to the Amazon SNS endpoint\.
+ Identifying the response sent from the Amazon SNS endpoint to Amazon SNS\.
+ Determining the message dwell time \(the time between the publish timestamp and just before handing off to an Amazon SNS endpoint\)\.

 To configure topic attributes for message delivery status, you can use the AWS Management Console, AWS software development kits \(SDKs\), or query API\. 

**Topics**
+ [Configuring delivery status logging using the AWS Management Console](#topics-attrib)
+ [Configuring message delivery status attributes for topics subscribed to Amazon SNS endpoints using the AWS SDKs](#msg-status-sdk)

## Configuring delivery status logging using the AWS Management Console<a name="topics-attrib"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and then choose **Edit**\.

1. On the **Edit *MyTopic*** page, expand the **Delivery status logging** section\.

1. Choose the protocol for which you want to log delivery status; for example **AWS Lambda**\.

1. Enter the **Success sample rate** \(the percentage of successful messages for which you want to receive CloudWatch Logs\.

1. In the **IAM roles** section, do one of the following:
   + To choose an existing service role from your account, choose **Use existing service role** and then specify IAM roles for successful and failed deliveries\.
   + To create a new service role in your account, choose **Create new service role**, choose **Create new roles** to define the IAM roles for successful and failed deliveries in the IAM console\.

     To give Amazon SNS write access to use CloudWatch Logs on your behalf, choose **Allow**\.

1. Choose **Save changes**\.

   You can now view and parse the CloudWatch Logs containing the message delivery status\. For more information about using CloudWatch, see the [CloudWatch Documentation](https://aws.amazon.com/documentation/cloudwatch)\.

## Configuring message delivery status attributes for topics subscribed to Amazon SNS endpoints using the AWS SDKs<a name="msg-status-sdk"></a>

The AWS SDKs provide APIs in several languages for using message delivery status attributes with Amazon SNS\. 

### Topic attributes<a name="topic-attributes"></a>

You can use the following topic attribute name values for message delivery status:

**HTTP**
+ `HTTPSuccessFeedbackRoleArn`
+ `HTTPSuccessFeedbackSampleRate`
+ `HTTPFailureFeedbackRoleArn`

**Amazon Kinesis Data Firehose**
+ `FirehoseSuccessFeedbackRoleArn`
+ `FirehoseSuccessFeedbackSampleRate`
+ `FirehoseFailureFeedbackRoleArn`

**AWS Lambda**
+ `LambdaSuccessFeedbackRoleArn`
+ `LambdaSuccessFeedbackSampleRate`
+ `LambdaFailureFeedbackRoleArn`

**Platform application endpoint**
+ `ApplicationSuccessFeedbackRoleArn`
+ `ApplicationSuccessFeedbackSampleRate`
+ `ApplicationFailureFeedbackRoleArn`
**Note**  
In addition to being able to configure topic attributes for message delivery status of notification messages sent to Amazon SNS application endpoints, you can also configure application attributes for the delivery status of push notification messages sent to push notification services\. For more information, see [Using Amazon SNS Application Attributes for Message Delivery Status](https://docs.aws.amazon.com/sns/latest/dg/sns-msg-status.html)\. 

**Amazon SQS**
+ `SQSSuccessFeedbackRoleArn`
+ `SQSSuccessFeedbackSampleRate`
+ `SQSFailureFeedbackRoleArn`

 The `<ENDPOINT>SuccessFeedbackRoleArn` and `<ENDPOINT>FailureFeedbackRoleArn` attributes are used to give Amazon SNS write access to use CloudWatch Logs on your behalf\. The `<ENDPOINT>SuccessFeedbackSampleRate` attribute is for specifying the sample rate percentage \(0\-100\) of successfully delivered messages\. After you configure the `<ENDPOINT>FailureFeedbackRoleArn` attribute, then all failed message deliveries generate CloudWatch Logs\. 

### AWS SDK examples to configure topic attributes<a name="topic-attributes-sdks"></a>

The following code examples show how to set Amazon SNS topic attributes\.

------
#### [ Java ]

**SDK for Java 2\.x**  
  

```
    public static void setTopAttr(SnsClient snsClient, String attribute, String topicArn, String value) {

        try {

            SetTopicAttributesRequest request = SetTopicAttributesRequest.builder()
                .attributeName(attribute)
                .attributeValue(value)
                .topicArn(topicArn)
                .build();

            SetTopicAttributesResponse result = snsClient.setTopicAttributes(request);
            System.out.println("\n\nStatus was " + result.sdkHttpResponse().statusCode() + "\n\nTopic " + request.topicArn()
                + " updated " + request.attributeName() + " to " + request.attributeValue());

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
+  For API details, see [SetTopicAttributes](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/SetTopicAttributes) in *AWS SDK for Java 2\.x API Reference*\. 

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
import {SetTopicAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  AttributeName: "ATTRIBUTE_NAME", // ATTRIBUTE_NAME
  TopicArn: "TOPIC_ARN", // TOPIC_ARN
  AttributeValue: "NEW_ATTRIBUTE_VALUE", //NEW_ATTRIBUTE_VALUE
};

const run = async () => {
  try {
    const data = await snsClient.send(new SetTopicAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sns#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sns-examples-managing-topics.html#sns-examples-managing-topicsstttopicattributes)\. 
+  For API details, see [SetTopicAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/settopicattributescommand.html) in *AWS SDK for JavaScript API Reference*\. 

------
#### [ Kotlin ]

**SDK for Kotlin**  
This is prerelease documentation for a feature in preview release\. It is subject to change\.
  

```
suspend fun setTopAttr(attribute: String?, topicArnVal: String?, value: String?) {

        val request = SetTopicAttributesRequest {
            attributeName = attribute
            attributeValue = value
            topicArn = topicArnVal
        }

       SnsClient { region = "us-east-1" }.use { snsClient ->
        snsClient.setTopicAttributes(request)
        println("Topic ${request.topicArn} was updated.")
       }
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/kotlin/services/secretsmanager#code-examples)\. 
+  For API details, see [SetTopicAttributes](https://github.com/awslabs/aws-sdk-kotlin#generating-api-documentation) in *AWS SDK for Kotlin API reference*\. 

------
#### [ PHP ]

**SDK for PHP**  
  

```
require 'vendor/autoload.php';

use Aws\Sns\SnsClient; 
use Aws\Exception\AwsException;

/**
 * Configure the message delivery status attributes for an Amazon SNS Topic.
 *
 * This code expects that you have AWS credentials set up per:
 * https://docs.aws.amazon.com/sdk-for-php/v3/developer-guide/guide_credentials.html
 */
 
$SnSclient = new SnsClient([
    'profile' => 'default',
    'region' => 'us-east-1',
    'version' => '2010-03-31'
]);
$attribute = 'Policy | DisplayName | DeliveryPolicy';
$value = 'First Topic';
$topic = 'arn:aws:sns:us-east-1:111122223333:MyTopic';

try {
    $result = $SnSclient->setTopicAttributes([
        'AttributeName' => $attribute,
        'AttributeValue' => $value,
        'TopicArn' => $topic,
    ]);
    var_dump($result);
} catch (AwsException $e) {
    // output error message if fails
    error_log($e->getMessage());
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/php/example_code/sns#code-examples)\. 
+  For API details, see [SetTopicAttributes](https://docs.aws.amazon.com/goto/SdkForPHPV3/sns-2010-03-31/SetTopicAttributes) in *AWS SDK for PHP API Reference*\. 

------
#### [ Ruby ]

**SDK for Ruby**  
  

```
require 'aws-sdk-sns'  # v2: require 'aws-sdk'

policy  = '{
  "Version":"2008-10-17",
  "Id":"__default_policy_ID",
  "Statement":[{
    "Sid":"__default_statement_ID",
    "Effect":"Allow",
    "Principal":{
      "AWS":"*"
    },
    "Action":["SNS:Publish"],
    "Resource":"' + MY_TOPIC_ARN + '",
    "Condition":{
      "ArnEquals":{
        "AWS:SourceArn":"' + MY_RESOURCE_ARN + '"}
     }
  }]
}'
# Replace us-west-2 with the AWS Region you're using for Amazon SNS.
sns = Aws::SNS::Resource.new(region: 'REGION')

# Get topic by ARN
topic = sns.topic()

# Add policy to topic
topic.set_attributes({
  attribute_name: "POLICY_NAME",
  attribute_value: policy
})
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/ruby/example_code/sns#code-examples)\. 
+  For more information, see [AWS SDK for Ruby Developer Guide](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/sns-example-enable-resource.html)\. 
+  For API details, see [SetTopicAttributes](https://docs.aws.amazon.com/goto/SdkForRubyV3/sns-2010-03-31/SetTopicAttributes) in *AWS SDK for Ruby API Reference*\. 

------