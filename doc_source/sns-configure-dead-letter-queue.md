# Configuring an Amazon SNS dead\-letter queue for a subscription<a name="sns-configure-dead-letter-queue"></a>

A dead\-letter queue is an Amazon SQS queue that an Amazon SNS subscription can target for messages that can't be delivered to subscribers successfully\. Messages that can't be delivered due to client errors or server errors are held in the dead\-letter queue for further analysis or reprocessing\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md) and [Amazon SNS message delivery retries](sns-message-delivery-retries.md)\.

This page shows how you can use the AWS Management Console, an AWS SDK, the AWS CLI, and AWS CloudFormation to configure a dead\-letter queue for an Amazon SNS subscription\.

## Prerequisites<a name="dead-letter-queue-prerequisites"></a>

Before you configure a dead\-letter queue, complete the following prerequisites:

1. [Create an Amazon SNS topic](sns-create-topic.md) named `MyTopic`\.

1. [Create an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html) named `MyEndpoint`, to be used as the endpoint for the Amazon SNS subscription\.

1. \(Skip for AWS CloudFormation\) [Subscribe the queue to the topic](sns-sqs-as-subscriber.md)\.

1. [Create another Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html) named `MyDeadLetterQueue`, to be used as the dead\-letter queue for the Amazon SNS subscription\.

1. To give Amazon SNS principal access to the Amazon SQS API action, set the following queue policy for `MyDeadLetterQueue`\.

   ```
   {
     "Statement": [{
       "Effect": "Allow",
       "Principal": {
         "Service": "sns.amazonaws.com"
       },
       "Action": "SQS:SendMessage",
       "Resource": "arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue",
       "Condition": {
         "ArnEquals": {
           "aws:SourceArn": "arn:aws:sns:us-east-2:123456789012:MyTopic"
         }
       }
     }]
   }
   ```

**Topics**
+ [Prerequisites](#dead-letter-queue-prerequisites)
+ [AWS Management Console](#configure-dead-letter-queue-aws-console)
+ [AWS SDKs](#configure-dead-letter-queue-aws-sdk)
+ [AWS CLI](#configure-dead-letter-queue-aws-cli)
+ [AWS CloudFormation](#configure-dead-letter-queue-aws-cloudformation)

## To configure a dead\-letter queue for an Amazon SNS subscription using the AWS Management Console<a name="configure-dead-letter-queue-aws-console"></a>

Before your begin this tutorial, make sure you complete the [prerequisites](#dead-letter-queue-prerequisites)\.

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. [Create an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html) or use an existing queue and note the ARN of the queue on the **Details** tab of the queue, for example:

   ```
   arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue
   ```
**Note**  
For a [FIFO topic](sns-fifo-topics.md), use an Amazon SQS FIFO queue as a dead\-letter queue for the Amazon SNS subscrption\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, select an existing subscription and then choose **Edit**\.

1. On the **Edit *1234a567\-bc89\-012d\-3e45\-6fg7h890123i*** page, expand the **Redrive policy \(dead\-letter queue\)** section, and then do the following:

   1. Choose **Enabled**\.

   1. Specify the ARN of an Amazon SQS queue\.

1. Choose **Save changes**\.

   Your subscription is configured to use a dead\-letter queue\.

## To configure a dead\-letter queue for an Amazon SNS subscription using an AWS SDK<a name="configure-dead-letter-queue-aws-sdk"></a>

Before you run this example, make sure that you complete the [prerequisites](#dead-letter-queue-prerequisites)\.

To use an AWS SDK, you must configure it with your credentials\. For more information, see [The shared config and credentials files](https://docs.aws.amazon.com/sdkref/latest/guide/creds-config-files.html) in the *AWS SDKs and Tools Reference Guide*\.

The following code example shows how to set an Amazon SQS queue as a dead\-letter queue for an Amazon SNS subscription\.

------
#### [ Java ]

**SDK for Java 1\.x**  
  

```
// Specify the ARN of the Amazon SNS subscription.
String subscriptionArn =
    "arn:aws:sns:us-east-2:123456789012:MyEndpoint:1234a567-bc89-012d-3e45-6fg7h890123i";

// Specify the ARN of the Amazon SQS queue to use as a dead-letter queue.
String redrivePolicy =
    "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue\"}";

// Set the specified Amazon SQS queue as a dead-letter queue
// of the specified Amazon SNS subscription by setting the RedrivePolicy attribute.
SetSubscriptionAttributesRequest request = new SetSubscriptionAttributesRequest()
    .withSubscriptionArn(subscriptionArn)
    .withAttributeName("RedrivePolicy")
    .withAttributeValue(redrivePolicy);
sns.setSubscriptionAttributes(request);
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/java/example_code/sns#code-examples)\. 

------

## To configure a dead\-letter queue for an Amazon SNS subscription using the AWS CLI<a name="configure-dead-letter-queue-aws-cli"></a>

Before your begin this tutorial, make sure you complete the [prerequisites](#dead-letter-queue-prerequisites)\.

1. Install and configure the AWS CLI\. For more information, see the [https://docs.aws.amazon.com/cli/latest/userguide/](https://docs.aws.amazon.com/cli/latest/userguide/)\.

1. Use the following command\.

   ```
   aws sns set-subscription-attributes \
   --subscription-arn arn:aws:sns:us-east-2:123456789012:MyEndpoint:1234a567-bc89-012d-3e45-6fg7h890123i
   --attribute-name RedrivePolicy
   --attribute-value "{\"deadLetterTargetArn\": \"arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue\"}"
   ```

## To configure a dead\-letter queue for an Amazon SNS subscription using AWS CloudFormation<a name="configure-dead-letter-queue-aws-cloudformation"></a>

Before your begin this tutorial, make sure you complete the [prerequisites](#dead-letter-queue-prerequisites)\.

1. Copy the following JSON code to a file named `MyDeadLetterQueue.json`\.

   ```
   {
     "Resources": {
       "mySubscription": {
         "Type" : "AWS::SNS::Subscription",
         "Properties" : {
           "Protocol": "sqs",
           "Endpoint": "arn:aws:sqs:us-east-2:123456789012:MyEndpoint",
           "TopicArn": "arn:aws:sns:us-east-2:123456789012:MyTopic",
           "RedrivePolicy": {
             "deadLetterTargetArn":
               "arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue"
           }
         }
       }
     }
   }
   ```

1. Sign in to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation/)\.

1. On the **Select Template** page, choose **Upload a template to Amazon S3**, choose your `MyDeadLetterQueue.json` file, and then choose **Next**\. 

1. On the **Specify Details** page, enter `MyDeadLetterQueue` for **Stack Name**, and then choose **Next**\. 

1. On the **Options** page, choose **Next**\.

1. On the **Review** page, choose **Create**\.

   AWS CloudFormation begins to create the `MyDeadLetterQueue` stack and displays the **CREATE\_IN\_PROGRESS** status\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.