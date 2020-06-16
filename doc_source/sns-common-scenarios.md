# Common Amazon SNS scenarios<a name="sns-common-scenarios"></a>

## Fanout<a name="SNSFanoutScenario"></a>

The "fanout" scenario is when an Amazon SNS message is sent to a topic and then replicated and pushed to multiple Amazon SQS queues, HTTP endpoints, or email addresses\. This allows for parallel asynchronous processing\. For example, you could develop an application that sends an Amazon SNS message to a topic whenever an order is placed for a product\. Then, the Amazon SQS queues that are subscribed to that topic would receive identical notifications for the new order\. The Amazon EC2 server instance attached to one of the queues could handle the processing or fulfillment of the order, while the other server instance could be attached to a data warehouse for analysis of all orders received\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fanout.png)

Another way to use "fanout" is to replicate data sent to your production environment with your test environment\. Expanding upon the previous example, you could subscribe yet another queue to the same topic for new incoming orders\. Then, by attaching this new queue to your test environment, you could continue to improve and test your application using data received from your production environment\. 

**Important**  
Make sure that you consider data privacy and security before you send any production data to your test environment\. 

For more information about sending Amazon SNS messages to Amazon SQS queues, see [Using Amazon SNS for system\-to\-system messaging with an Amazon SQS queue as a subscriber](sns-sqs-as-subscriber.md)\. For more information about sending Amazon SNS messages to HTTP/S endpoints, see [Using Amazon SNS for system\-to\-system messaging with an HTTP/s endpoint as a subscriberUsing Amazon SNS for system\-to\-system messaging with AWS Event Fork Pipelines as a subscriber](sns-http-https-endpoint-as-subscriber.md)\.

## Application and system alerts<a name="SNSAlertsScenario"></a>

Application and system alerts are notifications that are triggered by predefined thresholds and sent to specified users by SMS and/or email\. For example, since many AWS services use Amazon SNS, you can receive immediate notification when an event occurs, such as a specific change to your Amazon EC2 Auto Scaling group\. 

## Push email and text messaging<a name="SNSPushMessaging"></a>

Push email and text messaging are two ways to transmit messages to individuals or groups via email and/or SMS\. For example, you could use Amazon SNS to push targeted news headlines to subscribers by email or SMS\. Upon receiving the email or SMS text, interested readers could then choose to learn more by visiting a website or launching an application\. For more information about using Amazon SNS to send SMS notifications, see [Text messaging \(SMS\)](sns-mobile-phone-number-as-subscriber.md)\. 

## Mobile push notifications<a name="SNSMobilePushScenario"></a>

Mobile push notifications enable you to send messages directly to mobile apps\. For example, you could use Amazon SNS for sending notifications to an app, indicating that an update is available\. The notification message can include a link to download and install the update\. For more information about using Amazon SNS to send direct notification messages to mobile endpoints, see [Using Amazon SNS for user notifications with a mobile application as a subscriber \(mobile push\)](sns-mobile-application-as-subscriber.md) 

## Message durability<a name="SNSMessageDurability"></a>

Amazon SNS provides durable storage of all messages that it receives\. When Amazon SNS receives your Publish request, it stores multiple copies of your message to disk\. Before Amazon SNS confirms to you that it received your request, it stores the message in multiple isolated locations known as *Availability Zones*\. The message is stored in Availability Zones that are located within your chosen AWS Region, such as the US East \(N\. Virginia\) Region\. Although rare, should a failure occur in one Availability Zone, Amazon SNS remains operational, and the durability of your messages persists\.