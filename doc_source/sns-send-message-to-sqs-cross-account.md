# Sending Amazon SNS Messages to an Amazon SQS Queue in a Different Account<a name="sns-send-message-to-sqs-cross-account"></a>

You can publish a notification to an Amazon SNS topic with one or more subscriptions to Amazon SQS queues in another account\. You set up the topic and queues the same way you would if they were in the same account \(see [Using Amazon SNS for System\-to\-System Messaging with an Amazon SQS Queue as a Subscriber](sns-sqs-as-subscriber.md)\)\. The only difference is how you handle subscription confirmation, and that depends on how you subscribe the queue to the topic\.

**Topics**
+ [Queue Owner Creates Subscription](#SendMessageToSQS.cross.account.queueowner)
+ [User Who Does Not Own the Queue Creates Subscription](#SendMessageToSQS.cross.account.notqueueowner)

## Queue Owner Creates Subscription<a name="SendMessageToSQS.cross.account.queueowner"></a>

When the queue owner creates the subscription, the subscription does not require confirmation\. The queue starts receiving notifications from the topic as soon as the `Subscribe` action completes\. To enable the queue owner to subscribe to the topic owner's topic, the topic owner must give the queue owner's account permission to call the `Subscribe` action on the topic\. When added to the topic MyTopic in the account `123456789012`, the following policy gives the account `111122223333` permission to call `sns:Subscribe` on MyTopic in the account 123456789012\.

```
{
      "Version":"2012-10-17",
      "Id":"MyTopicSubscribePolicy",
      "Statement":[{
          "Sid":"Allow-other-account-to-subscribe-to-topic",
          "Effect":"Allow",
          "Principal":{
            "AWS":"111122223333"
          },
          "Action":"sns:Subscribe",
          "Resource":"arn:aws:sns:us-east-1:123456789012:MyTopic"
        }
      ]
    }
```

**To set the policy**

1. Choose **Topics** and choose your topic's ARN\.

1. On the **Topic details: MyTopic** page, choose **Other topic actions**, **Edit topic policy**\.

1. In the **Edit topic policy** dialog box, choose **Advanced view**, enter the policy, and choose **Update**\.

After this policy has been set on MyTopic, a user can log in to the Amazon SNS console with credentials for account 111122223333 to subscribe to the topic\.

**To add an Amazon SQS queue subscription to a topic in another account using the Amazon SQS console**

1. Using the credentials of the AWS account containing the queue or an IAM user in that account, sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/](https://console.aws.amazon.com/sns/)\.

1. Make sure you have the ARNs for both the topic and the queue\. You will need them when you create the subscription\.

1. Make sure you have set `sqs:SendMessage` permission on the queue so that it can receive messages from the topic\. For more information, see [Step 2: Give Permission to the Amazon SNS Topic to Send Messages to the Amazon SQS Queue](sns-sqs-as-subscriber.md#SendMessageToSQS.sqs.permissions)\.

1. On the navigation panel, choose the **SNS Dashboard**\.

1. In the **Dashboard**, in the **Additional Actions** section, choose **Create New Subscription**\. 

1. In the **Topic ARN** box, enter the ARN for the topic\.

1. For **Protocol**, choose **Amazon SQS**\.

1. In the **Endpoint** box, enter the ARN for the queue\. 

1. Choose **Subscribe**\.

1. For the **Subscription request received\!** message, you'll notice text that says you must confirm the subscription\. Because you are the queue owner, the subscription does not need to be confirmed\. Choose **Close**\. You've completed the subscription process and notification messages published to the topic can now be sent to the queue\.

 The user can also use the access key and secret key for the AWS account 111122223333 to issue the `sns-subscribe` command or call the `[Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html)` API action to subscribe an Amazon SQS queue to MyTopic in the account 123456789012\. The following `[sns\-subscribe](https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html)` CLI command subscribes the queue MyQ from account 111122223333 to the topic MyTopic in account 123456789012\.

```
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic --protocol sqs --notification-endpoint arn:aws:sqs:us-east-1:111122223333:MyQ
```

**Note**  
To be able to send, the queue must have permissions for Amazon SNS\.

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