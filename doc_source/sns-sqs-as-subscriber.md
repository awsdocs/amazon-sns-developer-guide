# Using Amazon SNS for system\-to\-system messaging with an Amazon SQS queue as a subscriber<a name="sns-sqs-as-subscriber"></a>

[Amazon SNS](https://aws.amazon.com/sns/) works closely with Amazon Simple Queue Service \(Amazon SQS\)\. Both services provide different benefits for developers\. Amazon SNS allows applications to send time\-critical messages to multiple subscribers through a “push” mechanism, eliminating the need to periodically check or “poll” for updates\. Amazon SQS is a message queue service used by distributed applications to exchange messages through a polling model, and can be used to decouple sending and receiving components—without requiring each component to be concurrently available\. Using Amazon SNS and Amazon SQS together, messages can be delivered to applications that require immediate notification of an event, and also persisted in an Amazon SQS queue for other applications to process at a later time\. 

When you subscribe an Amazon SQS queue to an Amazon SNS topic, you can publish a message to the topic and Amazon SNS sends an Amazon SQS message to the subscribed queue\. The Amazon SQS message contains the subject and message that were published to the topic along with metadata about the message in a JSON document\. The Amazon SQS message will look similar to the following JSON document\.

```
{
   "Type" : "Notification",
   "MessageId" : "63a3f6b6-d533-4a47-aef9-fcf5cf758c76",
   "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
   "Subject" : "Testing publish to subscribed queues",
   "Message" : "Hello world!",
   "Timestamp" : "2012-03-29T05:12:16.901Z",
   "SignatureVersion" : "1",
   "Signature" : "EXAMPLEnTrFPa3...",
   "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
   "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:c7fe3a54-ab0e-4ec2-88e0-db410a0f2bee"
}
```

Amazon SNS isn't currently compatible with Amazon SQS FIFO queues\.

## Subscribing an Amazon SQS queue to an Amazon SNS topic<a name="subscribe-sqs-queue-to-sns-topic"></a>

To enable an Amazon SNS topic to send messages to an Amazon SQS queue, do one of the following:
+ Use the [Amazon SQS console](https://console.aws.amazon.com/sqs/), which simplifies the process\. For more information, see [Subscribing an Amazon SQS queue to an Amazon SNS topic](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-subscribe-queue-sns-topic.html) in the *Amazon Simple Queue Service Developer Guide*\.
+ Follow these steps: 

  1. [Get the Amazon Resource Name \(ARN\) of the queue you want to send messages to and the topic to which you want to subscribe the queue\.](#SendMessageToSQS.arn)

  1. [Give `sqs:SendMessage` permission to the Amazon SNS topic so that it can send messages to the queue\.](#SendMessageToSQS.sqs.permissions)

  1. [Subscribe the queue to the Amazon SNS topic\.](#SendMessageToSQS.subscribe)

  1. [Give IAM users or AWS accounts the appropriate permissions to publish to the Amazon SNS topic and read messages from the Amazon SQS queue\.](#SendMessageToSQS.iam.permissions)

  1. [Test it out by publishing a message to the topic and reading the message from the queue\.](#SendMessageToSQS.test)

To learn about how to set up a topic to send messages to a queue that is in a different AWS account, see [Sending Amazon SNS messages to an Amazon SQS queue in a different account](sns-send-message-to-sqs-cross-account.md)\.

To see an AWS CloudFormation template that creates a topic that sends messages to two queues, see [Using an AWS CloudFormation template to create a topic that sends messages to Amazon SQS queues](SendMessageToSQS.cloudformation.md)\.

### Step 1: Get the ARN of the queue and topic<a name="SendMessageToSQS.arn"></a>

When subscribing a queue to your topic, you'll need a copy of the ARN for the queue\. Similarly, when giving permission for the topic to send messages to the queue, you'll need a copy of the ARN for the topic\.

To get the queue ARN, you can use the Amazon SQS console or the [GetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/Query_QueryGetQueueAttributes.html) API action\.

**To get the queue ARN from the Amazon SQS console**

1. Sign in to the AWS Management Console and open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. Select the box for the queue whose ARN you want to get\.

1. From the **Details** tab, copy the ARN value so that you can use it to subscribe to the Amazon SNS topic\.

To get the topic ARN, you can use the Amazon SNS console, the `[sns\-get\-topic\-attributes](https://docs.aws.amazon.com/cli/latest/reference/sns/get-topic-attributes.html)` command, or the `[GetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/Query_QueryGetQueueAttributes.html)` API action\.

**To get the topic ARN from the Amazon SNS console**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose the topic whose ARN you want to get\.

1. From the **Topic Details** section, copy the **Topic ARN** value so that you can use it to give permission for the Amazon SNS topic to send messages to the queue\.

### Step 2: Give permission to the Amazon SNS topic to send messages to the Amazon SQS queue<a name="SendMessageToSQS.sqs.permissions"></a>

For an Amazon SNS topic to be able to send messages to a queue, you must set a policy on the queue that allows the Amazon SNS topic to perform the `sqs:SendMessage` action\.

Before you subscribe a queue to a topic, you need a topic and a queue\. If you haven't already created a topic or queue, create them now\. For more information, see [Creating a Topic](https://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html), and see [Creating a Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-getting-started.htmlCreatingQueue.html) in the *Amazon Simple Queue Service Developer Guide*\.

To set a policy on a queue, you can use the Amazon SQS console or the [SetQueueAttributes](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/Query_QuerySetQueueAttributes.html) API action\. Before you start, make sure you have the ARN for the topic that you want to allow to send messages to the queue\.

**To set a SendMessage policy on a queue using the Amazon SQS console**

1. Sign in to the AWS Management Console and open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. Select the box for the queue whose policy you want to set, choose the **Permissions** tab, and then choose **Add a Permission**\.

1. In the **Add a Permission** dialog box, select **Allow** for **Effect**, choose **Everybody \(\*\)** for **Principal**, and then select **SendMessage** from the **Actions** drop\-down\.

1. Add a condition that allows the action for the topic\. Choose **Add Conditions \(optional\)**, choose **ArnEquals** for **Condition**, choose **aws:SourceArn** for **Key**, and paste in the topic ARN for **Value**\. Choose **Add Condition**\. The new condition should appear at the bottom of the box \(you may have to scroll down to see this\)\.

1. Choose **Add Permission**\.

1. Choose **Edit Policy Document \(Advanced\)**\.

1. Set `Principal` to be the Amazon SNS service, as shown in the example below\. 

If you wanted to create the policy document yourself, you would create a policy like the following\. The policy allows MyTopic to send messages to MyQueue\. 

```
{
  "Statement": [{
    "Effect":"Allow",
    "Principal": {
      "Service": "sns.amazonaws.com"
    },
    "Action":"sqs:SendMessage",
    "Resource":"arn:aws:sqs:us-east-2:123456789012:MyQueue",
    "Condition":{
      "ArnEquals":{
        "aws:SourceArn":"arn:aws:sns:us-east-2:123456789012:MyTopic"
      }
    }
  }]
}
```

### Step 3: Subscribe the queue to the Amazon SNS topic<a name="SendMessageToSQS.subscribe"></a>

To send messages to a queue through a topic, you must subscribe the queue to the Amazon SNS topic\. You specify the queue by its ARN\. To subscribe to a topic, you can use the Amazon SNS console, the `[sns\-subscribe](https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html)` CLI command, or the `[Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html)` API action\. Before you start, make sure you have the ARN for the queue that you want to subscribe\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic\.

1. On the ***MyTopic*** page, in the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. Verify the **Topic ARN**\.

   1. For **Protocol**, choose **Amazon SQS**\.

   1. For **Endpoint**, enter the ARN of an Amazon SQS queue\.

   1. Choose **Create Subscription**\.

   When the subscription is confirmed, your new subscription's **Subscription ID** displays its subscription ID\. If the owner of the queue creates the subscription, the subscription is automatically confirmed and the subscription should be active almost immediately\.

   Usually, you'll be subscribing your own queue to your own topic in your own account\. However, you can also subscribe a queue from a different account to your topic\. If the user who creates the subscription is not the owner of the queue \(for example, if a user from account A subscribes a queue from account B to a topic in account A\), the subscription must be confirmed\. For more information about subscribing a queue from a different account and confirming the subscription, see [Sending Amazon SNS messages to an Amazon SQS queue in a different account](sns-send-message-to-sqs-cross-account.md)\.

### Step 4: Give users permissions to the appropriate topic and queue actions<a name="SendMessageToSQS.iam.permissions"></a>

You should use AWS Identity and Access Management \(IAM\) to allow only appropriate users to publish to the Amazon SNS topic and to read/delete messages from the Amazon SQS queue\. For more information about controlling actions on topics and queues for IAM users, see [Using identity\-based policies with Amazon SNS](sns-using-identity-based-policies.md), and [Controlling User Access to Your AWS Account ](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/UsingIAM.html) in the Amazon Simple Queue Service Developer Guide\.

There are two ways to control access to a topic or queue:
+ [Add a policy to an IAM user or group](#SendMessageToSQS.iam.permissions.user)\. The simplest way to give users permissions to topics or queues is to create a group and add the appropriate policy to the group and then add users to that group\. It's much easier to add and remove users from a group than to keep track of which policies you set on individual users\.
+ [Add a policy to topic or queue](#SendMessageToSQS.iam.permissions.resource)\. If you want to give permissions to a topic or queue to another AWS account, the only way you can do that is by adding a policy that has as its principal the AWS account you want to give permissions to\.

You should use the first method for most cases \(apply policies to groups and manage permissions for users by adding or removing the appropriate users to the groups\)\. If you need to give permissions to a user in another account, you should use the second method\.

#### Adding a policy to an IAM user or group<a name="SendMessageToSQS.iam.permissions.user"></a>

If you added the following policy to an IAM user or group, you would give that user or members of that group permission to perform the `sns:Publish` action on the topic MyTopic\.

```
{
  "Statement": [{
    "Effect": "Allow",
    "Action": "sns:Publish",
    "Resource": "arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```

If you added the following policy to an IAM user or group, you would give that user or members of that group permission to perform the `sqs:ReceiveMessage` and `sqs:DeleteMessage` actions on the queues MyQueue1 and MyQueue2\.

```
{
  "Statement":[{
    "Effect":"Allow",
    "Action":[
      "sqs:ReceiveMessage",
      "sqs:DeleteMessage"
    ],
    "Resource":[
      "arn:aws:sns:us-east-2:123456789012:MyQueue1",
      "arn:aws:sns:us-east-2:123456789012:MyQueue2"
    ]
  }]
}
```

#### Adding a policy to a topic or queue<a name="SendMessageToSQS.iam.permissions.resource"></a>

The following example policies show how to give another account permissions to a topic and queue\.

**Note**  
When you give another AWS account access to a resource in your account, you are also giving IAM users who have admin\-level access \(wildcard access\) permissions to that resource\. All other IAM users in the other account are automatically denied access to your resource\. If you want to give specific IAM users in that AWS account access to your resource, the account or an IAM user with admin\-level access must delegate permissions for the resource to those IAM users\. For more information about cross\-account delegation, see [Enabling Cross\-Account Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/Delegation.html) in the *Using IAM Guide*\.

If you added the following policy to a topic MyTopic in account 123456789012, you would give account 111122223333 permission to perform the `sns:Publish` action on that topic\.

```
{
  "Statement":[{
    "Effect":"Allow",
    "Principal":{
      "AWS":"111122223333"
    },
    "Action":"sns:Publish",
    "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```

If you added the following policy to a queue MyQueue in account 123456789012, you would give account 111122223333 permission to perform the `sqs:ReceiveMessage` and `sqs:DeleteMessage` actions on that queue\.

```
{
  "Statement":[{
    "Effect":"Allow",
    "Principal":{
      "AWS":"111122223333"
    },
    "Action":[
      "sqs:DeleteMessage",
      "sqs:ReceiveMessage"
    ],
    "Resource":[
      "arn:aws:sns:us-east-2:123456789012:MyQueue"
    ]
  }]
}
```

### Step 5: Test the topic's queue subscriptions<a name="SendMessageToSQS.test"></a>

You can test a topic's queue subscriptions by publishing to the topic and viewing the message that the topic sends to the queue\.

**To publish to a topic using the Amazon SNS console**

1. Using the credentials of the AWS account or IAM user with permission to publish to the topic, sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose the topic and choose **Publish to Topic**\.

1. In the **Subject** box, enter a subject \(for example, **Testing publish to queue**\) in the **Message** box, enter some text \(for example, **Hello world\!**\), and choose **Publish Message**\. The following message appears: Your message has been successfully published\.

**To view the message from the topic using the Amazon SQS console**

1. Using the credentials of the AWS account or IAM user with permission to view messages in the queue, sign in to the AWS Management Console and open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. Check the box for the queue that is subscribed to the topic\.

1. From the **Queue Action** drop\-down, choose **View/Delete Messages** and choose **Start Polling for Messages**\. A message with a type of **Notification** appears\. 

1. In the **Body** column, choose **More Details**\. The **Message Details** box contains a JSON document that contains the subject and message that you published to the topic\. The message looks similar to the following JSON document\.

   ```
   {
     "Type" : "Notification",
     "MessageId" : "63a3f6b6-d533-4a47-aef9-fcf5cf758c76",
     "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
     "Subject" : "Testing publish to subscribed queues",
     "Message" : "Hello world!",
     "Timestamp" : "2012-03-29T05:12:16.901Z",
     "SignatureVersion" : "1",
     "Signature" : "EXAMPLEnTrFPa3...",
     "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
     "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:c7fe3a54-ab0e-4ec2-88e0-db410a0f2bee"
   }
   ```

1. Choose **Close**\. You have successfully published to a topic that sends notification messages to a queue\.