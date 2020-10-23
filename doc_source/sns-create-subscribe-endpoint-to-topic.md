# Subscribing to an Amazon SNS topic<a name="sns-create-subscribe-endpoint-to-topic"></a>

To receive messages published to [a topic](sns-create-topic.md), you must *subscribe* an endpoint \(such as AWS Lambda, Amazon SQS, HTTP/S, or an email address\) to the topic\. When you subscribe an endpoint to a topic and confirm the subscription, the endpoint begins to receive messages published to the associated topic\.

**Topics**
+ [AWS Management Console](#subscribe-topic-aws-console)
+ [AWS SDK for Java](#create-subscribe-endpoint-to-topic-aws-java)
+ [AWS SDK for \.NET](#create-subscribe-endpoint-to-topic-aws-dot-net)

## To subscribe an endpoint to a Amazon SNS topic using the AWS Management Console<a name="subscribe-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, choose the Amazon Resource Name \(ARN\) of a topic\.

   1. For **Protocol**, choose an endpoint type, such as **Email**\. To subscribe to a [FIFO topic](sns-fifo-topics.md), choose **Amazon SQS** for the protocol\.

   1. For **Endpoint**, enter the endpoint value, such as an email address or the ARN of an Amazon SQS queue\. Note that HTTP\(S\) endpoints, email addresses, and AWS resources in other AWS accounts require confirmation\.

   1. \(Optional\) To configure a filter policy, expand the **Subscription filter policy** section\. For more information, see [Filter policy constraints](sns-subscription-filter-policies.md#subscription-filter-policy-constraints)\.

   1. \(Optional\) To configure a dead\-letter queue for the topic, expand the **Redrive policy \(dead\-letter queue\)** section\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md)\.

   1. Choose **Create subscription**\.

      The console creates the subscription and opens the subscription's **Details** page\.

For some endpoint types—such as HTTP\(S\) endpoints, email addresses, and AWS resources in other AWS accounts—you must confirm the subscription before the endpoint can start to receive messages\.

**To confirm a subscription**

1. If your endpoint is an email address, check your email inbox and choose **Confirm subscription** in the email from Amazon SNS\.

1. Amazon SNS opens your web browser and displays a subscription confirmation with your subscription ID\.

## To subscribe an endpoint to an Amazon SNS topic using the AWS SDK for Java<a name="create-subscribe-endpoint-to-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   The following code excerpt creates a subscription for an email endpoint and then prints the `SubscribeRequest` request ID\.

   ```
   // Subscribe an email endpoint to an Amazon SNS topic.
   final SubscribeRequest subscribeRequest = new SubscribeRequest(topicArn, "email", "name@example.com");
   snsClient.subscribe(subscribeRequest);
   
   // Print the request ID for the SubscribeRequest action.
   System.out.println("SubscribeRequest: " + snsClient.getCachedResponseMetadata(subscribeRequest));
   System.out.println("To confirm the subscription, check your email.");
   ```

1. Compile and run your code\.

   The subscription is created and the `SubscribeRequest` request ID is printed, for example:

   ```
   SubscribeRequest: {AWS_REQUEST_ID=1234a567-bc89-012d-3e45-6fg7h890123i}
   To confirm the subscription, check your email.
   ```

For a detailed example of how to create and publish a FIFO topic using the AWS SDK for Java, see [Using the AWS SDK for Java 2\.x](fifo-topic-code-examples.md#fifo-topic-java)\.

## To subscribe an endpoint to an Amazon SNS topic using the AWS SDK for \.NET<a name="create-subscribe-endpoint-to-topic-aws-dot-net"></a>

1. Specify your AWS credentials\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

1. Write your code\. For more information, see [Programming with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-programming-techniques.html)\.

   The following code excerpt creates a subscription for an email endpoint and then prints the `SubscribeRequest` request ID\.

   ```
   // Subscribe an email endpoint to an Amazon SNS topic.
   SubscribeRequest subscribeRequest = new SubscribeRequest(topicArn, "email", "name@example.com");
   SubscribeResponse subscribeResponse = snsClient.Subscribe(subscribeRequest);
   
   // Print the request ID for the SubscribeRequest action.
   Console.WriteLine("SubscribeRequest: " + subscribeResponse.ResponseMetadata.RequestId);
   Console.WriteLine("To confirm the subscription, check your email.");
   ```

1. Compile and run your code\.

   The subscription is created and the `SubscribeRequest` request ID is printed, for example:

   ```
   SubscribeRequest: 1234a567-bc89-012d-3e45-6fg7h890123i
   To confirm the subscription, check your email.
   ```