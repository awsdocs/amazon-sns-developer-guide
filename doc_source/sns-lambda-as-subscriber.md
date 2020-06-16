# Using Amazon SNS for system\-to\-system messaging with an AWS Lambda function as a subscriber<a name="sns-lambda-as-subscriber"></a>

Amazon SNS and AWS Lambda are integrated so you can invoke Lambda functions with Amazon SNS notifications\. When a message is published to an SNS topic that has a Lambda function subscribed to it, the Lambda function is invoked with the payload of the published message\. The Lambda function receives the message payload as an input parameter and can manipulate the information in the message, publish the message to other SNS topics, or send the message to other AWS services\. 

In addition, Amazon SNS also supports message delivery status attributes for message notifications sent to Lambda endpoints\. For more information, see [Amazon SNS message delivery status](sns-topic-attributes.md)\. 

## Prerequisites<a name="lambda-prereq"></a>

To invoke Lambda functions using Amazon SNS notifications, you need the following:
+ Lambda function
+ Amazon SNS topic

For information about creating a Lambda function to use with Amazon SNS, see [Using Lambda with Amazon SNS](https://docs.aws.amazon.com/lambda/latest/dg/with-sns-example.html)\. For information about creating an Amazon SNS topic, see [Create a topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.

 When you use Amazon SNS to deliver messages from opt\-in regions to regions which are enabled by default, you must alter the policy created in the AWS Lambda function by replacing the principal `sns.amazonaws.com` with `sns.<opt-in-region>.amazonaws.com`\. 

 For example, if you want to subscribe an AWS Lambda function in US East \(N\. Virginia\) to an SNS topic in Asia Pacific \(Hong Kong\), change the principal in the AWS Lambda function policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Middle East \(Bahrain\), EU \(Milano\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\. 

**Note**  
We do not support cross\-region delivery to AWS Lambda from a region that is enabled by default to an opt\-in region\. Also, cross\-region forwarding of SNS messages from opt\-in regions to other opt\-in regions is not supported\. 

## Configuring Amazon SNS with Lambda endpoints using the AWS Management Console<a name="lambda-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic\.

1. In the **Subscriptions** section, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. Verify the chosen **Topic ARN**\.

   1. For **Protocol** choose AWS Lambda\.

   1. For **Endpoint** enter the ARN of a function\.

   1. Choose **Create subscription**\.

When a message is published to an SNS topic that has a Lambda function subscribed to it, the Lambda function is invoked with the payload of the published message\. For information about how to use AWS Lambda with Amazon SNS, including a tutorial, see [ Using AWS Lambda with Amazon SNS](https://docs.aws.amazon.com/lambda/latest/dg/with-sns.html)\.