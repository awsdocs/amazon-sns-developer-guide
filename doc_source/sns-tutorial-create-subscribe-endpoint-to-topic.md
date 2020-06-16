# Tutorial: Subscribing an endpoint to an Amazon SNS topic<a name="sns-tutorial-create-subscribe-endpoint-to-topic"></a>

To receive messages published to [a topic](sns-tutorial-create-topic.md), you must *subscribe* an endpoint \(such as AWS Lambda, Amazon SQS, HTTP/S, or an email address\) to the topic\. When you subscribe an endpoint to a topic and confirm the subscription, the endpoint begins to receive messages published to the associated topic\.

The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to create a subscription and then subscribe an endpoint to a topic\.

**Topics**
+ [AWS Management Console](#create-subscribe-endpoint-to-topic-aws-console)
+ [AWS SDK for Java](#create-subscribe-endpoint-to-topic-aws-java)
+ [AWS SDK for \.NET](#create-subscribe-endpoint-to-topic-aws-dot-net)

## To subscribe an endpoint to an Amazon SNS topic using the AWS Management Console<a name="create-subscribe-endpoint-to-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, do the following:

   1. Enter the **Topic ARN** of the topic you created earlier, for example:

      ```
      arn:aws:sns:us-east-2:123456789012:MyTopic
      ```
**Note**  
To see a list of the topics in the current AWS account, choose the **Topic ARN** field\.

   1. For **Protocol**, choose an endpoint type, for example **Email**\.

   1. For **Endpoint**, enter an email address that can receive notifications, for example:

      ```
      name@example.com
      ```
**Note**  
After your subscription is created, you must confirm it\. Only HTTP/S endpoints, email addresses, and AWS resources in other AWS accounts require confirmation\. \(Amazon SQS queues and Lambda functions in the same AWS account—as well as mobile endpoints —don't require confirmation\.\)

   1. Choose **Create subscription**\.

      The subscription is created and the ***Subscription: 1234a567\-bc89\-012d\-3e45\-6fg7h890123i*** page is displayed\.

      The subscription's **ARN**, **Endpoint**, **Topic**, **Status** \(**Pending confirmation** at this stage\), and **Protocol** are displayed in the **Details** section\.

1. In your email client, check the email address that you specified and choose **Confirm subscription** in the email from Amazon SNS\.

1. In your web browser, a subscription confirmation with your subscription ID is displayed\.

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