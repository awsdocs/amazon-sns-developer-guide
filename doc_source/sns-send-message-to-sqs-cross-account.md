# Sending Amazon SNS messages to an Amazon SQS queue in a different account<a name="sns-send-message-to-sqs-cross-account"></a>

You can publish a notification to an Amazon SNS topic with one or more subscriptions to Amazon SQS queues in another account\. You set up the topic and queues the same way you would if they were in the same account \(see [Using Amazon SNS for system\-to\-system messaging with an Amazon SQS queue as a subscriber](sns-sqs-as-subscriber.md)\)\. The major difference is how you handle subscription confirmation, and that depends on how you subscribe the queue to the topic\.

**Topics**
+ [Opt\-in regions](#SendMessageToSQS.regions)
+ [Queue owner creates subscription](#SendMessageToSQS.cross.account.queueowner)
+ [A user who does not own the queue creates subscription](#SendMessageToSQS.cross.account.notqueueowner)

## Opt\-in regions<a name="SendMessageToSQS.regions"></a>

 When you use Amazon SNS to deliver messages from opt\-in regions to regions which are enabled by default, you must alter the resource policy created for the queue\. Replace the principal `sns.amazonaws.com` with `sns.<opt-in-region>.amazonaws.com`\. 

 For example, if you want to subscribe a queue in US East \(N\. Virginia\) to an SNS topic in Asia Pacific \(Hong Kong\), change the principal in the queue policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Middle East \(Bahrain\), EU \(Milano\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\. 

**Note**  
We also support cross\-region delivery to Amazon SQS from a region that is enabled by default to an opt\-in region\. However, cross\-region forwarding of SNS messages from opt\-in regions to other opt\-in regions is not supported\. 

## Queue owner creates subscription<a name="SendMessageToSQS.cross.account.queueowner"></a>

The account that created the Amazon SQS queue is the queue owner\. When the queue owner creates a subscription, the subscription doesn't require confirmation\. The queue begins to receive notifications from the topic as soon as the `Subscribe` action completes\. To let the queue owner subscribe to the topic owner's topic, the topic owner must give the queue owner's account permission to call the `Subscribe` action on the topic\.

### Step 1: To set the topic policy using the AWS Management Console<a name="sns-tutorial-set-topic-policy"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. Select a topic and then choose **Edit**\.

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

### Step 2: To add an Amazon SQS queue subscription to a topic in another AWS account using the AWS Management Console<a name="sns-tutorial-add-sqs-subscription-to-sns-topic-another-account"></a>

Before you begin, make sure you have the ARNs for your topic and queue and that you have [given permission to the topic to send messages to the queue](sns-sqs-as-subscriber.md#SendMessageToSQS.sqs.permissions)\.

1. On the navigation panel, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**

1. On the **Create subscription** page, in the **Details** section, do the following:

   1. For **Topic ARN**, enter the ARN of the topic\.

   1. For **Protocol**, choose **Amazon SQS**\.

   1. For **Endpoint**, enter the ARN of the queue\.

   1. Choose **Create subscription**\.
**Note**  
To be able to communicate with the service, the queue must have permissions for Amazon SNS\.
Because you are the owner of the queue, you don't have to confirm the subscription\.

## A user who does not own the queue creates subscription<a name="SendMessageToSQS.cross.account.notqueueowner"></a>

Any user who creates a subscription but isn't the owner of the queue must confirm the subscription\.

When you use the `Subscribe` action, Amazon SNS sends a subscription confirmation to the queue\. The subscription is displayed in the Amazon SNS console, with its subscription ID set to **Pending Confirmation**\.

To confirm the subscription, a user with permission to read messages from the queue must visit the subscription URL\. Until the subscription is confirmed, no notifications published to the topic are sent to the queue\. To confirm the subscription, you can use the Amazon SQS console or the `[ReceiveMessage](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/Query_QueryReceiveMessage.html)` action\.

**Note**  
Before you subscribe an endpoint to the topic, make sure that the queue can receive messages from the topic by setting the `sqs:SendMessage` permission for the queue\. For more information, see [Step 2: Give permission to the Amazon SNS topic to send messages to the Amazon SQS queue](sns-sqs-as-subscriber.md#SendMessageToSQS.sqs.permissions)\.

### To confirm a subscription using the AWS Management Console<a name="sns-tutorial-confirm-subscription-console"></a>

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. Select the queue that has a pending subscription to the topic\.

1. Choose **Queue Actions**, **View/Delete Messages** and then choose **Start Polling for Messages**\.

   A message with the subscription confirmation is received in the queue\.

1. In the **Body** column, do the following:

   1. Choose **More Details**\.

   1. In the **Message Details** dialog box, find and note the **SubscribeURL** value, for example:

      ```
      https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-east-2:123456789012:MyTopic&Token=2336412f37fb...
      ```

1. In a web browser, navigate to the URL\.

   An XML response is displayed, for example:

   ```
   <ConfirmSubscriptionResponse>
      <ConfirmSubscriptionResult>
         <SubscriptionArn>arn:aws:sns:us-east-2:123456789012:MyTopic:1234a567-bc89-012d-3e45-6fg7h890123i</SubscriptionArn>
      </ConfirmSubscriptionResult>
      <ResponseMetadata>
         <RequestId>abcd1efg-23hi-jkl4-m5no-p67q8rstuvw9</RequestId>
      </ResponseMetadata>
   </ConfirmSubscriptionResponse>
   ```

   The subscribed queue is ready to receive messages from the topic\.

1. \(Optional\) If you view the topic subscription in the Amazon SNS console, you can see that the **Pending Confirmation** message has been replaced by the subscription ARN in the **Subscription ID** column\.