# Tutorial: Deleting an Amazon SNS subscription and topic<a name="sns-tutorial-delete-subscription-topic"></a>

When you no longer need a subscription or topic, you must first unsubscribe from the topic before you can delete the topic\.

The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to subscriptions and topics\.

**Topics**
+ [AWS Management Console](#delete-subscription-topic-aws-console)
+ [AWS SDK for Java](#delete-subscription-topic-aws-java)
+ [AWS SDK for \.NET](#delete-subscription-topic-aws-dot-net)

## To delete an Amazon SNS subscription and topic using the AWS Management Console<a name="delete-subscription-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose a *confirmed* subscription and then choose **Delete**\.
**Note**  
You can't delete a pending confirmation\. After 3 days, Amazon SNS deletes it automatically\.

1. In the **Delete subscription** dialog box, choose **Delete**\.

   The subscription is deleted\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and then choose **Delete**\.
**Important**  
When you delete a topic, you also delete all subscriptions to the topic\.

1. On the **Delete topic *MyTopic*** dialog box, enter `delete me` and then choose **Delete**\.

   The topic is deleted\.

## To delete an Amazon SNS subscription and topic using the AWS SDK for Java<a name="delete-subscription-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

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