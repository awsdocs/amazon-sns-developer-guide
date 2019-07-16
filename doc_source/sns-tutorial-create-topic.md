# Tutorial: Creating an Amazon SNS Topic<a name="sns-tutorial-create-topic"></a>

An Amazon SNS topic is a logical access point which acts as a *communication channel*\. A topic lets you group multiple *endpoints* \(such as AWS Lambda, Amazon SQS, HTTP/S, or an email address\)\.

To broadcast the messages of a message\-producer system \(for example, an e\-commerce website\) working with multiple other services that require its messages \(for example, checkout and fulfillment systems\), you can create a topic for your producer system\.

The first and most common Amazon SNS task is creating a topic\. The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to create a topic\.

**Topics**
+ [AWS Management Console](#create-topic-aws-console)
+ [AWS SDK for Java](#create-topic-aws-java)
+ [AWS SDK for \.NET](#create-topic-aws-dot-net)

## To Create a Topic Using the AWS Management Console<a name="create-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. Do one of the following:
   + If no topics have ever been created under your AWS account before, read the description of Amazon SNS on the home page\.
   + If topics have been created under your AWS account before, on the navigation panel, choose **Topics**\.

1. In the **Create topic** section, enter a **Topic name**, for example *MyTopic*\.

1. \(Optional\) Expand the **Encryption** section and do the following\. For more information, see [Protecting Amazon SNS Data Using Server\-Side Encryption \(SSE\) and AWS KMS](sns-server-side-encryption.md)\.

   1. Choose **Enable encryption**\.

   1. Specify the customer master key \(CMK\)\. For more information, see [Key Terms](sns-server-side-encryption.md#sse-key-terms)\.

      For each CMK type, the **Description**, **Account**, and **CMK ARN** are displayed\.
**Important**  
If you aren't the owner of the CMK, or if you log in with an account that doesn't have the `kms:ListAliases` and `kms:DescribeKey` permissions, you won't be able to view information about the CMK on the Amazon SNS console\.  
Ask the owner of the CMK to grant you these permissions\. For more information, see the [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.
      + The AWS managed CMK for Amazon SNS **\(Default\) alias/aws/sns** is selected by default\.
**Note**  
Keep the following in mind:  
The first time you use the AWS Management Console to specify the AWS managed CMK for Amazon SNS for a topic, AWS KMS creates the AWS managed CMK for Amazon SNS\.
Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed CMK for Amazon SNS\.
      + To use a custom CMK from your AWS account, choose the **Customer master key \(CMK\)** field and then select the custom CMK from the list\.
**Note**  
For instructions on creating custom CMKs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*
      + To use a custom CMK ARN from your AWS account or from another AWS account, enter it into the **Customer master key \(CMK\)** field\.

1. \(Optional\) To configure access permissions for your topic, expand the **Access policy** section\. For more information, see [Authentication and Access Control for Amazon SNS](sns-authentication-and-access-control.md)\.

1. \(Optional\) To configure how Amazon SNS retries failed message delivery attempts, expand the **Delivery retry policy \(HTTP/S\)** section\. For more information, see [Setting Amazon SNS Delivery Retry Policies for HTTP/HTTPS Endpoints](DeliveryPolicies.md)\.

1. \(Optional\) To configure how Amazon SNS logs the delivery of messages to CloudWatch, expand the **Delivery status logging** section\. For more information, see [Amazon SNS Message Delivery Status](sns-topic-attributes.md)\.

1. \(Optional\) To add metadata tags to the topic, expand the **Tags** section, enter a **Key** and a **Value** \(optional\) and choose **Add tag**\. For more information, see [Amazon SNS Tags](sns-tags.md)\.

1. Choose **Create topic**\.

   The topic is created and the ***MyTopic*** page is displayed\.

   The topic's **Name**, **ARN**, \(optional\) **Display name**, and **Topic owner**'s AWS account ID are displayed in the **Details** section\.

1. Copy the topic ARN to the clipboard, for example:

   ```
   arn:aws:sns:us-east-2:123456789012:MyTopic
   ```

## To Create a Topic Using the AWS SDK for Java<a name="create-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   The following code excerpt creates the topic *`MyTopic`* and then prints the topic ARN and the `CreateTopicRequest` request ID for a previously executed successful request\.

   ```
   // Create an Amazon SNS topic.
   final CreateTopicRequest createTopicRequest = new CreateTopicRequest("MyTopic");
   final CreateTopicResponse createTopicResponse = snsClient.createTopic(createTopicRequest);
   
   // Print the topic ARN.
   System.out.println("TopicArn:" + createTopicResponse.getTopicArn());
       
   // Print the request ID for the CreateTopicRequest action.
   System.out.println("CreateTopicRequest: " + snsClient.getCachedResponseMetadata(createTopicRequest));
   ```

1. Compile and run your code\.

   The topic is created and the topic ARN and `CreateTopicRequest` request ID are printed, for example:

   ```
   TopicArn: arn:aws:sns:us-east-2:123456789012:MyTopic
   CreateTopicRequest: {AWS_REQUEST_ID=1234a567-bc89-012d-3e45-6fg7h890123i}
   ```

1. You can assign the topic ARN to a String variable to use in additional operations, for example:

   ```
   final String topicArn = "arn:aws:sns:us-east-2:123456789012:MyTopic";
   ```

## To Create a Topic Using the AWS SDK for \.NET<a name="create-topic-aws-dot-net"></a>

1. Specify your AWS credentials\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

1. Write your code\. For more information, see [Programming with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/latest/developer-guide/net-dg-programming-techniques.html)\.

   The following code excerpt creates the topic *`MyTopic`* and then prints the topic ARN and the `CreateTopicRequest` request ID\.

   ```
   // Create an Amazon SNS topic.
   CreateTopicRequest createTopicRequest = new CreateTopicRequest("MyTopic");
   CreateTopicResponse createTopicResponse = snsClient.CreateTopic(createTopicRequest);
   
   // Print the topic ARN.
   Console.WriteLine("TopicArn: " + createTopicResponse.TopicArn);
   
   // Print the request ID for the CreateTopicRequest action.
   Console.WriteLine("CreateTopicRequest: " + createTopicResponse.ResponseMetadata.RequestId);
   ```

1. Compile and run your code\.

   The topic is created and the topic ARN and `CreateTopicRequest` request ID are printed, for example:

   ```
   TopicArn: arn:aws:sns:us-east-2:123456789012:MyTopic
   CreateTopicRequest: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```

1. You can assign the topic ARN to a String variable to use in additional operations, for example:

   ```
   String topicArn = createTopicResponse.TopicArn;
   ```