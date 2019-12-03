# Sending Amazon SNS Messages to an Amazon SQS Queue in a Different Account<a name="sns-send-message-to-sqs-cross-account"></a>

You can publish a notification to an Amazon SNS topic with one or more subscriptions to Amazon SQS queues in another account\. You set up the topic and queues the same way you would if they were in the same account \(see [Using Amazon SNS for System\-to\-System Messaging with an Amazon SQS Queue as a Subscriber](sns-sqs-as-subscriber.md)\)\. The only difference is how you handle subscription confirmation, and that depends on how you subscribe the queue to the topic\.

**Topics**
+ [Queue Owner Creates Subscription](#SendMessageToSQS.cross.account.queueowner)
+ [User Who Does Not Own the Queue Creates Subscription](#SendMessageToSQS.cross.account.notqueueowner)

## Queue Owner Creates Subscription<a name="SendMessageToSQS.cross.account.queueowner"></a>

When the queue owner creates a subscription, the subscription doesn't require confirmation\. The queue starts begins to receive notifications from the topic as soon as the `Subscribe` action completes\. To let the queue owner subscribe to the topic owner's topic, the topic owner must give the queue owner's account permission to call the `Subscribe` action on the topic\.

### Step 1: To Set the Topic Policy Using the AWS Management Console<a name="sns-tutorial-set-topic-policy"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics**\.

1. Select a topic—for example, **MyTopic**—and then choose **Edit**\.

1. On the **Edit *MyTopic*** page, expand the **Access policy** section\.

1. Enter the following policy:

   ```
   {
     "Statement":[{
       "Effect":"Allow",
       "Principal":{
         "AWS":"111122223333"
       },
       "Action":"sns:Subscribe",
       "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
     }]
   }
   ```

   This policy gives account `111122223333` permission to call `sns:Subscribe` on `MyTopic` in account 123456789012\.

1. Choose **Save changes\.**

   A user with the credentials for account 111122223333 can subscribe to MyTopic\.

### Step 2: To Add an Amazon SQS Queue Subscription to a Topic in Another AWS Account Using the AWS Management Console<a name="sns-tutorial-add-sqs-subscription-to-sns-topic-another-account"></a>

Before you begin, make sure you have the ARNs for your topic and queue and that you have [given permission to the topic to send messages to the queue](sns-sqs-as-subscriber.md#SendMessageToSQS.sqs.permissions)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, enter the ARN of the topic\.

   1. For **Protocol**, choose **Amazon SQS**\.

   1. For **Endtpoint**, enter the ARN of the queue\.

   1. Choose **Create subscription**\.
**Note**  
To be able to communicate with the service, the queue must have permissions for Amazon SNS\.
Because you are the owner of the queue, you don't have to confirm the subscription\.

## User Who Does Not Own the Queue Creates Subscription<a name="SendMessageToSQS.cross.account.notqueueowner"></a>

When a user who is not the queue owner creates the subscription \(for example, when the topic owner in account A adds a subscription for a queue in account B\), the subscription must be confirmed\.

**Important**  
Before you subscribe to the topic, make sure you have set `sqs:SendMessage` permission on the queue so that it can receive messages from the topic\. See [Step 2: Give Permission to the Amazon SNS Topic to Send Messages to the Amazon SQS Queue](sns-sqs-as-subscriber.md#SendMessageToSQS.sqs.permissions)\.

When the user calls the `Subscribe` action, a message of type `SubscriptionConfirmation` is sent to the queue and the subscription is displayed in the Amazon SNS console with its Subscription ID set to **Pending Confirmation**\. To confirm the subscription, a user who can read messages from the queue must visit the URL specified in the `SubscribeURL` value in the message\. Until the subscription is confirmed, no notifications published to the topic are sent to the queue\. To confirm a subscription, you can use the Amazon SQS console or the [ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/Query_QueryReceiveMessage.html) API action\.

**To confirm a subscription using the Amazon SQS console**

1. Sign in to the AWS Management Console and open the Amazon SQS console at [https://console\.aws\.amazon\.com/sqs/](https://console.aws.amazon.com/sqs/)\.

1. Select the queue that has a pending subscription to the topic\.

1. From the **Queue Action** drop\-down, choose **View/Delete Messages** and choose **Start Polling for Messages**\. A message with a type of **SubscriptionConfirmation** appears\. 

1. In the **Body** column, choose **More Details**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sqs-confirm0.png)

1. In the text box, find the **SubscribeURL** value and copy the URL\. It will look similar to the following URL\.

   ```
   https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37fb687f5d51e6e241d09c805d352fe148e56f8cff30f023ff35db8bccbc62721725b074841be6524bb215b0c45ec571ba1e7faacc309940c0b4b9e511ab85eba671412a4c314ecd446127ff1a9cfe08642b8e3738e73c279dd3ae565bd98f842ed992a4742ebec0946ebd9a
   ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sqs-confirm.png)

1. In a web browser, paste the URL into the address bar to visit the URL\. You will see a response similar to the following XML document\.

   ```
   <ConfirmSubscriptionResponse xmlns="http://sns.amazonaws.com/doc/2010-03-31/">
          <ConfirmSubscriptionResult>
             <SubscriptionArn>arn:aws:sns:us-west-2:123456789012:MyTopic:c7fe3a54-ab0e-4ec2-88e0-db410a0f2bee</SubscriptionArn>
          </ConfirmSubscriptionResult>
          <ResponseMetadata>
             <RequestId>dd266ecc-7955-11e1-b925-5140d02da9af</RequestId>
          </ResponseMetadata>
       </ConfirmSubscriptionResponse>
   ```

   If you view the topic subscription in the Amazon SNS console, you will now see that subscription ARN replaces the **Pending Confirmation** message in the **Subscription ID** column\. The subscribed queue is ready to receive messages from the topic\.