# Getting started with Amazon SNS<a name="sns-getting-started"></a>

This section helps you become more familiar with Amazon SNS by showing you how to manage topics, subscriptions, and messages using the AWS Management Console\.

## Prerequisites<a name="sns-prerequisites"></a>

Before you begin, complete the steps in [Setting up access for Amazon SNS](sns-setting-up.md)\.

## Step 1: Create a topic<a name="step-create-queue"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the **Create topic** section, enter a **Topic name**, for example *MyTopic*\.

1. Choose **Create topic**\.

   The topic is created and the ***MyTopic*** page is displayed\.

   The topic's **Name**, **ARN**, \(optional\) **Display name**, and **Topic owner**'s AWS account ID are displayed in the **Details** section\.

1. Copy the topic ARN to the clipboard, for example:

   ```
   arn:aws:sns:us-east-2:123456789012:MyTopic
   ```

## Step 2: Create a subscription for an endpoint to the topic<a name="step-send-message"></a>

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

## Step 3: Publish a message to the topic<a name="step-receive-delete-message"></a>

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose the topic you created earlier and then choose **Publish message**\.

1. On the **Publish message to topic** page, do the following:

   1. \(Optional\) In the **Message details** section, enter the **Subject**, for example:

      ```
      Hello from Amazon SNS!
      ```

   1. In the **Message body** section, do one of the following:
      + Choose **Identical payload for all delivery protocols** and then enter the message, for example:

        ```
        Publishing a message to an Amazon SNS topic.
        ```
      + Choose **Custom payload for each delivery protocol** and then use a JSON object to define the message to send to each protocol, for example:

        ```
        {
          "default": "Sample fallback message",
          "email": "Sample message for email endpoints",
          "sqs": "Sample message for Amazon SQS endpoints",
          "lambda": "Sample message for AWS Lambda endpoints",
          "http": "Sample message for HTTP endpoints",
          "https": "Sample message for HTTPS endpoints",
          "sms": "Sample message for SMS endpoints",
          "APNS": "{\"aps\":{\"alert\": \"Sample message for iOS endpoints\"} }",
          "APNS_SANDBOX": "{\"aps\":{\"alert\":\"Sample message for iOS development endpoints\"}}",
          "APNS_VOIP": "{\"aps\":{\"alert\":\"Sample message for Apple VoIP endpoints\"}}",
          "APNS_VOIP_SANDBOX": "{\"aps\":{\"alert\": \"Sample message for Apple VoIP development endpoints\"} }",
          "MACOS": "{\"aps\":{\"alert\":\"Sample message for MacOS endpoints\"}}",
          "MACOS_SANDBOX": "{\"aps\":{\"alert\": \"Sample message for MacOS development endpoints\"} }",
          "GCM": "{ \"data\": { \"message\": \"Sample message for Android endpoints\" } }",
          "ADM": "{ \"data\": { \"message\": \"Sample message for FireOS endpoints\" } }",
          "BAIDU": "{\"title\":\"Sample message title\",\"description\":\"Sample message for Baidu endpoints\"}",
          "MPNS": "<?xml version=\"1.0\" encoding=\"utf-8\"?><wp:Notification xmlns:wp=\"WPNotification\"><wp:Tile><wp:Count>ENTER COUNT</wp:Count><wp:Title>Sample message for Windows Phone 7+ endpoints</wp:Title></wp:Tile></wp:Notification>",
          "WNS": "<badge version=\"1\" value=\"42\"/>"
        }
        ```

        For more information, see [Send custom platform\-specific payloads to mobile devices](sns-send-custom-platform-specific-payloads-mobile-devices.md)\.

   1. In the **Message attributes** section, add any attributes that you want Amazon SNS to match with the subscription attribute `FilterPolicy` to decide whether the subscribed endpoint is interested in the published message\.

      1. Select an attribute **Type**, for example **String\.Array**\.
**Note**  
If the attribute type is **String\.Array**, enclose the array in square brackets \(`[]`\)\. Within the array, enclose string values in double quotation marks\. You don't need quotation marks for numbers or for the keywords `true`, `false`, and `null`\.

      1. Enter a **Name** for the attribute, for example `customer_interests`\.

      1. Enter a **Value** for the attribute, for example `["soccer", "rugby", "hockey"]`\.

      If the attribute type is **String**, **String\.Array**, or **Number**, Amazon SNS evaluates the message attribute against a subscription's filter policy \(if present\) before sending the message to the subscription\.

      For more information, see [Amazon SNS message attributes](sns-message-attributes.md)\.

   1. Choose **Publish message**\.

      The message is published to the topic and the ***MyTopic*** page is displayed\.

      The topic's **Name**, **ARN**, \(optional\) **Display name**, and **Topic owner**'s AWS account ID are displayed in the **Details** section\.

1. In your email client, check the email address that you specified earlier and read the email from Amazon SNS\.

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

Now that you've created a topic and a subscription and learned how to send messages to a topic and how to delete a subscription and topic, you might want to try the following:
+ [Enable server\-side encryption for a topic\.](sns-tutorial-enable-encryption-for-topic.md)
+ [Enable server\-side encryption for a topic with an encrypted Amazon SQS queue subscribed\.](sns-tutorial-enable-encryption-for-topic-sqs-queue-subscriptions.md)
+ [Subscribe AWS Event Fork Pipelines to an Amazon SNS topic\.](sns-tutorial-subscribe-event-fork-pipelines.md)
+ [Deploy and test the AWS Event Fork Pipelines sample application](sns-tutorial-deploy-test-fork-pipelines-sample-application.md)\.
+ [Learn about message filtering\.](sns-message-filtering.md)
+ Learn how to interact with Amazon SNS programmatically by exploring the [AWS Developer Center](https://aws.amazon.com/developer/)\.
+ Learn about keeping an eye on costs and resources in the [Troubleshooting Amazon SNS topics](sns-troubleshooting.md) section\.
+ Learn about protecting your data and access to it in the [Security](sns-security.md) section\.