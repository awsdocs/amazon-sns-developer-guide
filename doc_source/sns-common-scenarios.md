# Common Amazon SNS scenarios<a name="sns-common-scenarios"></a>

## Application integration<a name="SNSFanoutScenario"></a>

The *Fanout* scenario is when a message published to an SNS topic is replicated and pushed to multiple endpoints, such as Kinesis Data Firehose delivery streams, Amazon SQS queues, HTTP\(S\) endpoints, and Lambda functions\. This allows for parallel asynchronous processing\.

For example, you can develop an application that publishes a message to an SNS topic whenever an order is placed for a product\. Then, SQS queues that are subscribed to the SNS topic receive identical notifications for the new order\. An Amazon Elastic Compute Cloud \(Amazon EC2\) server instance attached to one of the SQS queues can handle the processing or fulfillment of the order\. And you can attach another Amazon EC2 server instance to a data warehouse for analysis of all orders received\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fanout.png)

You can also use fanout to replicate data sent to your production environment with your test environment\. Expanding upon the previous example, you can subscribe another SQS queue to the same SNS topic for new incoming orders\. Then, by attaching this new SQS queue to your test environment, you can continue to improve and test your application using data received from your production environment\.

**Important**  
Make sure that you consider data privacy and security before you send any production data to your test environment\.

For more information, see the following resources:
+ [Fanout to Kinesis Data Firehose delivery streams](sns-firehose-as-subscriber.md)
+ [Fanout to Lambda functions](sns-lambda-as-subscriber.md)
+ [Fanout to Amazon SQS queues](sns-sqs-as-subscriber.md)
+ [Fanout to HTTP/S endpoints](sns-http-https-endpoint-as-subscriber.md)
+ [ Event\-Driven Computing with Amazon SNS and AWS Compute, Storage, Database, and Networking Services](https://aws.amazon.com/blogs/compute/event-driven-computing-with-amazon-sns-compute-storage-database-and-networking-services/) 

## Application alerts<a name="SNSAlertsScenario"></a>

Application and system alerts are notifications that are triggered by predefined thresholds\. Amazon SNS can send these notifications to specified users via SMS and email\. For example, you can receive immediate notification when an event occurs, such as a specific change to your Amazon EC2 Auto Scaling group, a new file uploaded to an Amazon S3 bucket, or a metric threshold breached in Amazon CloudWatch\. For more information, see [Setting up Amazon SNS notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) in the *Amazon CloudWatch User Guide*\.

## User notifications<a name="SNSPushMessaging"></a>

Amazon SNS can send push email messages and text messages \(SMS messages\) to individuals or groups\. For example, you could send e\-commerce order confirmations as user notifications\. For more information about using Amazon SNS to send SMS messages, see [Mobile text messaging \(SMS\)](sns-mobile-phone-number-as-subscriber.md)\.

## Mobile push notifications<a name="SNSMobilePushScenario"></a>

Mobile push notifications enable you to send messages directly to mobile apps\. For example, you can use Amazon SNS to send update notifications to an app\. The notification message can include a link to download and install the update\. For more information about using Amazon SNS to send push notification messages, see [Mobile push notifications](sns-mobile-application-as-subscriber.md)\.