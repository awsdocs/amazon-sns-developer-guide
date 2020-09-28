# Subscribing a function to a topic<a name="lambda-console"></a>

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