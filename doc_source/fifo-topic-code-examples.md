# Code examples for FIFO topics<a name="fifo-topic-code-examples"></a>

You can use the following code examples to integrate the [auto parts price management example use case](fifo-example-use-case.md) with SNS FIFO topics and SQS FIFO queues\. 

**Topics**
+ [FIFO example \(SDK for Java\)](#fifo-topic-java)
+ [FIFO example \(AWS CloudFormation\)](#fifo-topic-cfn)

## Using the AWS SDK for Java 2\.x<a name="fifo-topic-java"></a>

Using the AWS SDK for Java 2\.x, you create an Amazon SNS FIFO topic by setting its `FifoTopic` attribute to true\. You create an Amazon SQS FIFO queue by setting its `FifoQueue` attribute to true\. Also, you must add the `.fifo` suffix to the name of each FIFO resource\.

**Note**  
After you create a topic or queue resource as FIFO, you can't convert it into a standard topic or queue\.

To run these examples, follow the instructions in [ Getting Started with AWS SDK for Java 2\.0](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/getting-started.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

### Creating FIFO topics \(SDK for Java\)<a name="fifo-topic-java-create"></a>

 First, create the following FIFO resources:
+ The SNS FIFO topic that distributes the price updates
+ The SQS FIFO queues that provide these updates to the two applications \(wholesale and retail\)
+ The SNS FIFO subscriptions that connect both of the queues to the topic

**Note**  
If you test this code sample by publishing a message to the topic, make sure that you publish the message with the `business` atribute\. Specify either `retail` or `wholesale` for the attribute value\. Otherwise, the message is filtered out and not delivered to the subscribed queues\. 

```
// Create API clients

AWSCredentialsProvider credentials = getCredentials();

AmazonSNS sns = new AmazonSNSClient(credentials);
AmazonSQS sqs = new AmazonSQSClient(credentials);

// Create FIFO topic

Map<String, String> topicAttributes = new HashMap<String, String>();

topicAttributes.put("FifoTopic", "true");
topicAttributes.put("ContentBasedDeduplication", "false");

String topicArn = sns.createTopic(
    new CreateTopicRequest()
        .withName("PriceUpdatesTopic.fifo")
        .withAttributes(topicAttributes)
).getTopicArn();

// Create FIFO queues

Map<String, String> queueAttributes = new HashMap<String, String>();

queueAttributes.put("FifoQueue", "true");
queueAttributes.put("ContentBasedDeduplication", "false");

String wholesaleQueueUrl = sqs.createQueue(
    new CreateQueueRequest()
        .withName("WholesaleQueue.fifo")
        .withAttributes(queueAttributes)
).getQueueUrl();

String retailQueueUrl = sqs.createQueue(
    new CreateQueueRequest()
        .withName("RetailQueue.fifo")
        .withAttributes(queueAttributes)
).getQueueUrl();

// Subscribe FIFO queues to FIFO topic, setting required permissions

String wholesaleSubscriptionArn = 
    Topics.subscribeQueue(sns, sqs, topicArn, wholesaleQueueUrl);

String retailSubscriptionArn = 
    Topics.subscribeQueue(sns, sqs, topicArn, retailQueueUrl);
```

Note that content\-based deduplication has been disabled, because messages published with the same body might carry different attributes that must be processed independently\. The price management system uses the message attributes to define whether a given price update applies to the wholesale application, to the retail application, or to both\.

For more information about message deduplication, see [Message deduplication for FIFO topics](fifo-message-dedup.md)\.

### Setting filter policies for FIFO subscriptions \(SDK for Java\)<a name="fifo-topic-java-filter"></a>

After you create the SNS FIFO subscriptions, you can set their [filter policies](sns-subscription-filter-policies.md), so that each subscriber application receives only the price updates that it needs\. The following code example sets these policies, where the attribute represents the type of business, either wholesale or retail\. You can find the Java source code for the `SNSMessageFilterPolicy` class in [Subscription filter policies as Java collections](subscription-filter-policies-as-java-collections.md)\. For more information, see [Message filtering for FIFO topics](fifo-message-filtering.md)\.

```
// Set the Amazon SNS subscription filter polcies

SNSMessageFilterPolicy wholesalePolicy = new SNSMessageFilterPolicy();
wholesalePolicy.addAttribute("business", "wholesale");
wholesalePolicy.apply(sns, wholesaleSubscriptionArn);

SNSMessageFilterPolicy retailPolicy = new SNSMessageFilterPolicy();
retailPolicy.addAttribute("business", "retail");
retailPolicy.apply(sns, retailSubscriptionArn);
```

 

### Publishing messages to FIFO topics \(SDK for Java\)<a name="fifo-topic-java-publish"></a>

 

Now you're ready to publish messages to your Amazon SNS FIFO topic\. The following code example composes and publishes a wholesale price update message\.

```
// Publish message to FIFO topic 

String subject = "Price Update";
String payload = "{\"product\": 214, \"price\": 79.99}";
String groupId = "PID-214";
String dedupId = UUID.randomUUID().toString();
String attributeName = "business";
String attributeValue = "wholesale";

Map<String, MessageAttributeValue> attributes = new HashMap<>();

attributes.put(
    attributeName,
    new MessageAttributeValue()
        .withDataType("String")
        .withStringValue(attributeValue));

sns.publish(
    new PublishRequest()
        .withTopicArn(topicArn)
        .withSubject(subject)
        .withMessage(payload)
        .withMessageGroupId(groupId);
        .withMessageDeduplicationId(dedupId)
        .withMessageAttributes(attributes);
```

 

### Receiving messages from FIFO subscriptions<a name="fifo-receiving-messages"></a>

You can now receive price updates in the wholesale and retail applications\. As shown in the [FIFO topics example use case](fifo-example-use-case.md), the point of entry for each consumer application is the SQS FIFO queue, which its corresponding AWS Lambda function can poll automatically\. When an SQS FIFO queue is an event source for a Lambda function, Lambda scales its fleet of pollers as needed to efficiently consume messages\.

For more information, see [Using AWS Lambda with Amazon SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html) in the *AWS Lambda Developer Guide*\. For information on writing your own queue pollers, see [Recommendations for Amazon SQS standard and FIFO queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-best-practices.html#sqs-standard-fifo-queue-best-practices) in the *Amazon Simple Queue Service Developer Guide* and [ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) in the *Amazon Simple Queue Service API Reference*\.

## Using AWS CloudFormation<a name="fifo-topic-cfn"></a>

AWS CloudFormation enables you to use a template file to create and configure a collection of AWS resources together as a single unit\. This section has an example template that creates the following:
+ The SNS FIFO topic that distributes the price updates
+ The SQS FIFO queues that provide these updates to the two applications \(wholesale and retail\)
+ The SNS FIFO subscriptions that connect both of the queues to the topic
+ A [filter policy](sns-subscription-filter-policies.md) that specifies that subscriber applications receive only the price updates that they need 

**Note**  
If you test this code sample by publishing a message to the topic, make sure that you publish the message with the `business` atribute\. Specify either `retail` or `wholesale` for the attribute value\. Otherwise, the message is filtered out and not delivered to the subscribed queues\. 

```
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "PriceUpdatesTopic": {
      "Type": "AWS::SNS::Topic",
      "Properties": {
        "TopicName": "PriceUpdatesTopic.fifo",
        "FifoTopic": true,
        "ContentBasedDeduplication": false
      }
    },
    "WholesaleQueue": {
      "Type": "AWS::SQS::Queue",
      "Properties": {
        "QueueName": "WholesaleQueue.fifo",
        "FifoQueue": true,
        "ContentBasedDeduplication": false
      }
    },
    "RetailQueue": {
      "Type": "AWS::SQS::Queue",
      "Properties": {
        "QueueName": "RetailQueue.fifo",
        "FifoQueue": true,
        "ContentBasedDeduplication": false
      }
    },
    "WholesaleSubscription": {
      "Type": "AWS::SNS::Subscription",
      "Properties": {
        "TopicArn": {
          "Ref": "PriceUpdatesTopic"
        },
        "Endpoint": {
          "Fn::GetAtt": [
            "WholesaleQueue",
            "Arn"
          ]
        },
        "Protocol": "sqs",
        "RawMessageDelivery": "false",
        "FilterPolicy": {
          "business": [
            "wholesale"
          ]
        }
      }
    },
    "RetailSubscription": {
      "Type": "AWS::SNS::Subscription",
      "Properties": {
        "TopicArn": {
          "Ref": "PriceUpdatesTopic"
        },
        "Endpoint": {
          "Fn::GetAtt": [
            "RetailQueue",
            "Arn"
          ]
        },
        "Protocol": "sqs",
        "RawMessageDelivery": "false",
        "FilterPolicy": {
          "business": [
            "retail"
          ]
        }
      }
    },
    "SalesQueuesPolicy": {
      "Type": "AWS::SQS::QueuePolicy",
      "Properties": {
        "PolicyDocument": {
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "Service": "sns.amazonaws.com"
              },
              "Action": [
                "sqs:SendMessage"
              ],
              "Resource": "*",
              "Condition": {
                "ArnEquals": {
                  "aws:SourceArn": {
                    "Ref": "PriceUpdatesTopic"
                  }
                }
              }
            }
          ]
        },
        "Queues": [
          {
            "Ref": "WholesaleQueue"
          },
          {
            "Ref": "RetailQueue"
          }
        ]
      }
    }
  }
}
```

For more information about deploying AWS resources using an AWS CloudFormation template, see [Get Started](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/GettingStarted.Walkthrough.html) in the *AWS CloudFormation User Guide*\.