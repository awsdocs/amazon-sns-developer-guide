# Using Amazon SNS for System\-to\-System Messaging with an AWS Lambda Function as a Subscriber<a name="sns-lambda-as-subscriber"></a>

Amazon SNS and AWS Lambda are integrated so you can invoke Lambda functions with Amazon SNS notifications\. When a message is published to an SNS topic that has a Lambda function subscribed to it, the Lambda function is invoked with the payload of the published message\. The Lambda function receives the message payload as an input parameter and can manipulate the information in the message, publish the message to other SNS topics, or send the message to other AWS services\. 

In addition, Amazon SNS also supports message delivery status attributes for message notifications sent to Lambda endpoints\. For more information, see [Amazon SNS Message Delivery Status](sns-topic-attributes.md)\. 

## Prerequisites<a name="lambda-prereq"></a>

To invoke Lambda functions using Amazon SNS notifications, you need the following:
+ Lambda function
+ Amazon SNS topic

For information about creating a Lambda function, see [Getting Started with AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)\. For information about creating a Amazon SNS topic, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.

## Configuring Amazon SNS with Lambda Endpoints using the AWS Management Console<a name="lambda-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic\.

1. In the **Subscriptions** section, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. Verify the chosen **Topic ARN**\.

   1. For **Protocol** choose AWS Lambda\.

   1. For **Endpoint** enter the ARN of a function\.

   1. Choose **Create subscription**\.

When a message is published to an SNS topic that has a Lambda function subscribed to it, the Lambda function is invoked with the payload of the published message\. For information about how to create a sample message history store using SNS, Lambda, and Amazon DynamoDB, see the AWS Mobile Development blog [Invoking AWS Lambda functions via Amazon SNS](https://mobile.awsblog.com/post/Tx1VE917Z8J4UDY/Invoking-AWS-Lambda-functions-via-Amazon-SNS)\.