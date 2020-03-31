# Tutorial: Configuring an Amazon SNS dead\-letter queue for a subscription<a name="sns-configure-dead-letter-queue"></a>

A dead\-letter queue is an Amazon SQS queue that an Amazon SNS subscription can target for messages that can't be delivered to subscribers successfully\. Messages that can't be delivered due to client errors or server errors are held in the dead\-letter queue for further analysis or reprocessing\. For more information, see [Amazon SNS dead\-letter queues](sns-dead-letter-queues.md) and [Message delivery retries](sns-message-delivery-retries.md)\.

The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, the AWS CLI, and AWS CloudFormation to configure a dead\-letter queue for an Amazon SNS subscription\.

## Prerequisites<a name="dead-letter-queue-prerequisites"></a>

Before you begin any of the following tutorials, complete the following prerequisites:

1. [Create an Amazon SNS topic](sns-tutorial-create-topic.md) named `MyTopic`\.

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
+ [AWS SDK for Java](#configure-dead-letter-queue-aws-java)
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
Currently, you can't use an Amazon SQS FIFO queue as a dead\-letter queue for an Amazon SNS subscription\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, select an existing subscription and then choose **Edit**\.

1. On the **Edit *1234a567\-bc89\-012d\-3e45\-6fg7h890123i*** page, expand the **Redrive policy \(dead\-letter queue\)** section, and then do the following:

   1. Choose **Enabled**\.

   1. Specify the ARN of an Amazon SQS queue\.

1. Choose **Save changes**\.

   Your subscription is configured to use a dead\-letter queue\.

## To configure a dead\-letter queue for an Amazon SNS subscription using the AWS SDK for Java<a name="configure-dead-letter-queue-aws-java"></a>

Before your begin this tutorial, make sure you complete the [prerequisites](#dead-letter-queue-prerequisites)\.

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   For more information about creating Amazon SQS queues, see [To Configure an Amazon SQS Queue Using the AWS SDK for Java](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-create-queue.html#create-queue-java) in the *Amazon Simple Queue Service Developer Guide*\.

   The following code excerpt uses the ARN of an Amazon SNS subscription and an Amazon SQS queue to set the `RedrivePolicy` request parameter attribute\.

   ```
   // Specify the ARN of the Amazon SNS subscription.
   String subscriptionArn = 
       "arn:aws:sns:us-east-2:123456789012:MyEndpoint:1234a567-bc89-012d-3e45-6fg7h890123i";
   
   // Specify the ARN of the Amazon SQS queue to use as a dead-letter queue.
   String redrivePolicy = 
       "{\"deadLetterTargetArn\":\"arn:aws:sqs:us-east-2:123456789012:MyDeadLetterQueue\"}";
   
   // Set the specified Amazon SQS queue as a dead-letter queue 
   // of the specified Amazon SNS subscription.
   SetSubscriptionAttributesRequest request = new SetSubscriptionAttributesRequest()
       .withSubscriptionArn(subscriptionArn)
       .withAttributeName("RedrivePolicy")
       .withAttributeValue(redrivePolicy);
   sns.setSubscriptionAttributes(request);
   ```

1. Compile and run your code\.

   The Amazon SQS queue is set as the dead\-letter queue for the specified Amazon SNS subscription\.

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