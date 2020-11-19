# Email notifications<a name="sns-email-notifications"></a>

This page describes how to subscribe an [email address](#sns-email-notifications) to an Amazon SNS topic using the AWS Management Console, AWS SDK for Java, or AWS SDK for \.NET\. 

**Notes**  
You can't customize the body of the email message\. The email delivery feature is intended to provide internal system alerts, not marketing messages\.
Email delivery throughput is throttled according to [Amazon SNS quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html#limits_sns)\.

## To subscribe an email address to an Amazon SNS topic using the AWS Management Console<a name="create-subscribe-endpoint-to-topic-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, choose the Amazon Resource Name \(ARN\) of a topic\.

   1. For **Protocol**, choose **Email**\.

   1. For **Endpoint**, enter the email address\.

   1. \(Optional\) To configure a filter policy, expand the **Subscription filter policy** section\. For more information, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md)\.

   1. \(Optional\) To configure a dead\-letter queue for the subscription, expand the **Redrive policy \(dead\-letter queue\)** section\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md)\.

   1. Choose **Create subscription**\.

      The console creates the subscription and opens the subscription's **Details** page\.

You must confirm the subscription before the email address can start to receive messages\.

**To confirm a subscription**

1. Check your email inbox and choose **Confirm subscription** in the email from Amazon SNS\.

1. Amazon SNS opens your web browser and displays a subscription confirmation with your subscription ID\.

## To subscribe an email address to an Amazon SNS topic using the AWS SDK for Java<a name="create-subscribe-endpoint-to-topic-aws-java"></a>

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

## To subscribe an email address to an Amazon SNS topic using the AWS SDK for \.NET<a name="create-subscribe-endpoint-to-topic-aws-dot-net"></a>

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