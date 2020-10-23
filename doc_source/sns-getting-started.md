# Getting started with Amazon SNS<a name="sns-getting-started"></a>

This section helps you become more familiar with Amazon SNS by showing you how to manage topics, subscriptions, and messages using the Amazon SNS console\.

**Topics**
+ [Prerequisites](#sns-prerequisites)
+ [Step 1: Create a topic](#step-create-queue)
+ [Step 2: Create a subscription to the topic](#step-send-message)
+ [Step 3: Publish a message to the topic](#step-receive-delete-message)
+ [Step 4: Delete the subscription and topic](#step-delete-queue)
+ [Next steps](#sns-next-steps-getting-started)

## Prerequisites<a name="sns-prerequisites"></a>

Before you begin, complete the steps in [Setting up access for Amazon SNS](sns-setting-up.md)\.

## Step 1: Create a topic<a name="step-create-queue"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the left navigation pane, choose **Topics**\.

1. On the **Topics** page, choose **Create topic**\.

1. By default, the console creates a FIFO topic\. Choose **Standard**\.

1. In the **Details** section, enter a **Name** for the topic, such as *MyTopic*\.

1. Scroll to the end of the form and choose **Create topic**\.

   The console opens the new topic's **Details** page\.

## Step 2: Create a subscription to the topic<a name="step-send-message"></a>

1. In the left navigation pane, choose **Subscriptions**\.

1. On the **Subscriptions** page, choose **Create subscription**\.

1. On the **Create subscription** page, choose the **Topic ARN** field to see a list of the topics in your AWS account\.

1. Choose the topic that you created in the previous step\.

1. For **Protocol**, choose **Email**\.

1. For **Endpoint**, enter an email address that can receive notifications\.

1. Choose **Create subscription**\.

   The console opens the new subscription's **Details** page\.

1. Check your email inbox and choose **Confirm subscription** in the email from AWS Notifications\. The sender ID is usually "no\-reply@sns\.amazonaws\.com"\.

1. Amazon SNS opens your web browser and displays a subscription confirmation with your subscription ID\.

## Step 3: Publish a message to the topic<a name="step-receive-delete-message"></a>

1. In the left navigation pane, choose **Topics**\.

1. On the **Topics** page, choose the topic that you created earlier, and then choose **Publish message**\.

   The console opens the **Publish message to topic** page\.

1. \(Optional\) In the **Message details** section, enter a **Subject**, such as:

   ```
   Hello from Amazon SNS!
   ```

1. In the **Message body** section, choose **Identical payload for all delivery protocols**, and then enter a message body, such as:

   ```
   Publishing a message to an SNS topic.
   ```

1. Choose **Publish message**\.

   The message is published to the topic, and the console opens the topic's **Details** page\.

1. Check your email inbox and verify that you received an email from Amazon SNS with the published message\.

## Step 4: Delete the subscription and topic<a name="step-delete-queue"></a>

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

## Next steps<a name="sns-next-steps-getting-started"></a>

Now that you've created a topic with a subscription and sent messages to the topic, you might want to try the following:
+ Explore the [AWS Developer Center](https://aws.amazon.com/developer/)\.
+ Learn about protecting your data in the [Security](sns-security.md) section\.
+ Enable [server\-side encryption](sns-enable-encryption-for-topic.md) for a topic\.
+ Enable server\-side encryption for a topic with an [ encrypted Amazon Simple Queue Service \(Amazon SQS\) queue](sns-enable-encryption-for-topic-sqs-queue-subscriptions.md) subscribed\.
+ Subscribe [ AWS Event Fork Pipelines](sns-subscribe-event-fork-pipelines.md) to a topic\.