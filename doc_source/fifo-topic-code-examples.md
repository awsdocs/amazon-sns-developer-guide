# Code examples for FIFO topics<a name="fifo-topic-code-examples"></a>

You can use the following code examples to integrate the [auto parts price management example use case](fifo-example-use-case.md) with SNS FIFO topics and SQS FIFO queues\. 

**Topics**
+ [FIFO example \(AWS SDKs\)](#fifo-topic-aws-sdks)
+ [FIFO example \(AWS CloudFormation\)](#fifo-topic-cfn)

## Using an AWS SDK<a name="fifo-topic-aws-sdks"></a>

Using an AWS SDK, you create an Amazon SNS FIFO topic by setting its `FifoTopic` attribute to true\. You create an Amazon SQS FIFO queue by setting its `FifoQueue` attribute to true\. Also, you must add the `.fifo` suffix to the name of each FIFO resource\. After you create a FIFO topic or queue, you can't convert it into a standard topic or queue\.

The following code example creates these FIFO resources:
+ The SNS FIFO topic that distributes the price updates
+ The SQS FIFO queues that provide these updates to the two applications \(wholesale and retail\)
+ The SNS FIFO subscriptions that connect both of the queues to the topic

This example sets [filter policies](sns-subscription-filter-policies.md) on the subscriptions\. If you test the example by publishing a message to the topic, make sure that you publish the message with the `business` atribute\. Specify either `retail` or `wholesale` for the attribute value\. Otherwise, the message is filtered out and not delivered to the subscribed queues\. For more information, see [Message filtering for FIFO topics](fifo-message-filtering.md)\.

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
Create a FIFO topic and FIFO queues\. Subscribe the queues to the topic\.  

```
    public static void main(String[] args) {

        final String usage = "\n" +
            "Usage: " +
            "    <topicArn>\n\n" +
            "Where:\n" +
            "   fifoTopicName - The name of the FIFO topic. \n\n" +
            "   fifoQueueARN - The ARN value of a SQS FIFO queue. You can get this value from the AWS Management Console. \n\n";

        if (args.length != 2) {
            System.out.println(usage);
            System.exit(1);
        }

        String fifoTopicName = "PriceUpdatesTopic3.fifo";
        String fifoQueueARN = "arn:aws:sqs:us-east-1:814548047983:MyPriceSQS.fifo";
        SnsClient snsClient = SnsClient.builder()
            .region(Region.US_EAST_1)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();

        createFIFO(snsClient, fifoTopicName, fifoQueueARN);
    }

    public static void createFIFO(SnsClient snsClient, String topicName, String queueARN) {

        try {
            // Create a FIFO topic by using the SNS service client.
            Map<String, String> topicAttributes = new HashMap<>();
            topicAttributes.put("FifoTopic", "true");
            topicAttributes.put("ContentBasedDeduplication", "false");

            CreateTopicRequest topicRequest = CreateTopicRequest.builder()
                .name(topicName)
                .attributes(topicAttributes)
                .build();

            CreateTopicResponse response = snsClient.createTopic(topicRequest);
            String topicArn = response.topicArn();
            System.out.println("The topic ARN is"+topicArn);

            // Subscribe to the endpoint by using the SNS service client.
            // Only Amazon SQS FIFO queues can receive notifications from an Amazon SNS FIFO topic.
            SubscribeRequest subscribeRequest = SubscribeRequest.builder()
                .topicArn(topicArn)
                .endpoint(queueARN)
                .protocol("sqs")
                .build();

            snsClient.subscribe(subscribeRequest);
            System.out.println("The topic is subscribed to the queue.");

            // Compose and publish a message that updates the wholesale price.
            String subject = "Price Update";
            String payload = "{\"product\": 214, \"price\": 79.99}";
            String groupId = "PID-214";
            String dedupId = UUID.randomUUID().toString();
            String attributeName = "business";
            String attributeValue = "wholesale";

            MessageAttributeValue msgAttValue = MessageAttributeValue.builder()
                .dataType("String")
                .stringValue(attributeValue)
                .build();

            Map<String, MessageAttributeValue> attributes = new HashMap<>();
            attributes.put(attributeName, msgAttValue);
            PublishRequest pubRequest = PublishRequest.builder()
                .topicArn(topicArn)
                .subject(subject)
                .message(payload)
                .messageGroupId(groupId)
                .messageDeduplicationId(dedupId)
                .messageAttributes(attributes)
                .build();

            snsClient.publish(pubRequest);
            System.out.println("Message was published to "+topicArn);

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```

------

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