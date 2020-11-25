# Deleting an Amazon SNS subscription and topic<a name="sns-delete-subscription-topic"></a>

You can delete a subscription from an Amazon SNS topic, or you can delete the whole topic\. Note that you can't delete a subscription that's pending confirmation\. After three days, Amazon SNS deletes the unconfirmed subscription automatically\.

**Topics**
+ [AWS Management Console](#sns-delete-subscription-topic-console)
+ [AWS SDK for Java](#delete-subscription-topic-aws-java)
+ [AWS SDK for \.NET](#delete-subscription-topic-aws-dot-net)

## To delete an Amazon SNS subscription and topic using the AWS Management Console<a name="sns-delete-subscription-topic-console"></a>

**To delete a subscription using the AWS Management Console**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, select a subscription with a **Status** of **Confirmed**, and then choose **Delete**\.

1. In the **Delete subscription** dialog box, choose **Delete**\.

   The console deletes the subscription\.

When you delete a topic, Amazon SNS deletes the subscriptions associated with the topic\.

**To delete a topic using the AWS Management Console**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Topics**\.

1. On the **Topics** page, select a topic, and then choose **Delete**\.

1. In the **Delete topic** dialog box, enter `delete me`, and then choose **Delete**\.

   The console deletes the topic\.

## To delete an Amazon SNS subscription and topic using the AWS SDK for Java<a name="delete-subscription-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup.html#setup-credentials) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   The following code excerpt deletes a topic and then prints the `DeleteTopicRequest` request ID\.
**Important**  
When you delete a topic, you also delete all subscriptions to the topic\.

   ```
   // Delete an Amazon SNS topic.
   final DeleteTopicRequest deleteTopicRequest = new DeleteTopicRequest(topicArn);
   snsClient.deleteTopic(deleteTopicRequest);
   
   // Print the request ID for the DeleteTopicRequest action.
   System.out.println("DeleteTopicRequest: " + snsClient.getCachedResponseMetadata(deleteTopicRequest));
   ```

1. Compile and run your code\.

   The topic is deleted and the `DeleteTopicRequest` request ID is printed, for example:

   ```
   DeleteTopicRequest: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```

## To delete an Amazon SNS subscription and topic using the AWS SDK for \.NET<a name="delete-subscription-topic-aws-dot-net"></a>

1. Specify your AWS credentials\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

1. Write your code\. For more information, see [Programming with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-programming-techniques.html)\.

   The following code excerpt deletes a topic and then prints the `DeleteTopicRequest` request ID\.
**Important**  
When you delete a topic, you also delete all subscriptions to the topic\.

   ```
   // Delete an Amazon SNS topic.
   DeleteTopicRequest deleteTopicRequest = new DeleteTopicRequest(topicArn);
   DeleteTopicResponse deleteTopicResponse = snsClient.DeleteTopic(deleteTopicRequest);
   
   // Print the request ID for the DeleteTopicRequest action.
   Console.WriteLine("DeleteTopicRequest: " + deleteTopicResponse.ResponseMetadata.RequestId);
   ```

1. Compile and run your code\.

   The topic is deleted and the `DeleteTopicRequest` request ID is printed, for example:

   ```
   DeleteTopicRequest: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```