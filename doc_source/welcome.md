# What is Amazon SNS?<a name="welcome"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a managed service that provides message delivery from publishers to subscribers \(also known as *producers* and *consumers*\)\. Publishers communicate asynchronously with subscribers by sending messages to a *topic*, which is a logical access point and communication channel\. Clients can subscribe to the SNS topic and receive published messages using a supported protocol, such as Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages \(SMS\)\.

![\[A publisher sends a message to an Amazon SNS topic, and subscribers receive the message.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-how-works.png)

**Topics**
+ [Features and capabilities](#welcome-features)
+ [Related services](#welcome-related)
+ [Accessing Amazon SNS](#welcome-accessing)
+ [Pricing for Amazon SNS](#welcome-pricing)
+ [Common Amazon SNS scenarios](#sns-common-scenarios)

## Features and capabilities<a name="welcome-features"></a>

Amazon SNS provides the following features and capabilities:
+ **Application\-to\-application messaging** 

  Application\-to\-application messaging supports subscribers such as AWS Lambda functions, Amazon SQS queues, HTTP/S endpoints, and AWS Event Fork Pipelines\. For more information, see [Using Amazon SNS for application\-to\-application \(A2A\) messaging](sns-system-to-system-messaging.md)\. 
+ **Application\-to\-person notifications** 

  Application\-to\-person notifications provides user notifications to subscribers such as mobile applications, mobile phone numbers, and email addresses\. For more information, see [Using Amazon SNS for application\-to\-person \(A2P\) messaging](sns-user-notifications.md)\. 
+ **Message delivery retry** 

  Amazon SNS specifies a delivery policy for each delivery protocol\. The delivery policy defines how Amazon SNS retries the delivery of messages when server\-side errors occur\. For more information, see [Message delivery retries](sns-message-delivery-retries.md)\.
+ **Dead\-letter queues** 

  A dead\-letter queue is an Amazon SQS queue for messages that can't be delivered successfully due to client errors or server errors\. After a configurable number of retry attempts, an undeliverable message is held in the dead\-letter queue for further analysis or reprocessing\. For more information, see [Amazon SNS dead\-letter queues \(DLQs\)](sns-dead-letter-queues.md)\. 
+ **Message attributes** 

  Message attributes let you provide any arbitrary metadata about the message\. [Amazon SNS message attributes](sns-message-attributes.md)\. 
+ **Message filtering** 

  By default, each subscriber receives every message published to the topic\. To receive a subset of the messages, a subscriber must assign a filter policy to the topic subscription\. When the incoming message attributes match the filter policy attributes, the message is delivered to the subscribed endpoint\. Otherwise, the message is filtered out\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\. 
+ **Message security** 

  Server\-side encryption protects the contents of messages that are stored in Amazon SNS topics, using encryption keys provided by AWS KMS\. For more information, see [Encryption at rest](sns-server-side-encryption.md)\.

  You can also establish a private connection between Amazon SNS and your virtual private cloud \(VPC\)\. for more information, see [Internetwork traffic privacy](sns-internetwork-traffic-privacy.md)\.
+ **Message durability** 

  Amazon SNS provides durable storage of all messages that it receives\. When you publish a message to Amazon SNS, the service stores multiple copies of your message to disk\. Before Amazon SNS confirms to you that it received your request, it stores the message in multiple isolated locations known as *Availability Zones*\. The Availability Zones where your message is stored are located within your chosen AWS Region, such as the US East \(N\. Virginia\) Region\. Although rare, should a failure occur in one Availability Zone, Amazon SNS remains operational, and the durability of your messages persists\. 

## Related services<a name="welcome-related"></a>

You can use the following services with Amazon SNS:
+ **Amazon SQS** offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components\. Amazon SQS is related to Amazon SNS in the following ways:
  + Amazon SNS provides [dead\-letter queues](sns-dead-letter-queues.md) powered by Amazon SQS for undeliverable messages\.
  + You can [subscribe an Amazon SQS queue to an SNS topic](sns-sqs-as-subscriber.md)\.
+ **AWS Lambda** enables you to build applications that respond quickly to new information\. Run your application code in Lambda functions on highly available compute infrastructure\. For more information, see the [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/)\. You can [subscribe a Lambda function to an SNS topic](sns-lambda-as-subscriber.md)\.
+ **AWS Identity and Access Management \(IAM\)** helps you securely control access to AWS resources for your users\. Use IAM to control who can use your Amazon SNS topics \(authentication\), what topics they can use, and how they can use them \(authorization\)\. For more information, see [Using identity\-based policies with Amazon SNS](sns-using-identity-based-policies.md)\.
+ **AWS CloudFormation** enables you to model and set up your AWS resources\. Create a template that describes the AWS resources that you want, including Amazon SNS topics and subscriptions\. AWS CloudFormation takes care of provisioning and configuring those resources for you\. For more information, see the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)\.

## Accessing Amazon SNS<a name="welcome-accessing"></a>

You can configure and manage SNS topics and subscriptions using the Amazon SNS console, command line tools, or AWS SDKs\.
+ The [Amazon SNS console](https://console.aws.amazon.com/sns/v3/home) provides a convenient user interface for creating topics and subscriptions, sending and receiving messages, and monitoring events and logs\.
+ The AWS Command Line Interface \(AWS CLI\) gives you direct access to the Amazon SNS API for advanced configuration and automation use cases\. For more information, see [Using Amazon SNS with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-sns.html)\.
+ AWS provides SDKs in various languages\. For more information, see [SDKs and Toolkits](https://aws.amazon.com/getting-started/tools-sdks/)\.

## Pricing for Amazon SNS<a name="welcome-pricing"></a>

Amazon SNS has no upfront costs\. You pay based on the number of messages that you publish, the number of notifications that you deliver, and any additional API calls for managing topics and subscriptions\. Delivery pricing varies by endpoint type\. You can get started for free with the Amazon SNS free tier\.

For information, see [Amazon SNS pricing](https://aws.amazon.com/sns/pricing/)\.

## Common Amazon SNS scenarios<a name="sns-common-scenarios"></a>

### Application integration<a name="SNSFanoutScenario"></a>

The *Fanout* scenario is when a message published to an SNS topic is replicated and pushed to multiple endpoints, such as Amazon SQS queues, HTTP\(S\) endpoints, and Lambda functions\. This allows for parallel asynchronous processing\.

For example, you can develop an application that publishes a message to an SNS topic whenever an order is placed for a product\. Then, SQS queues that are subscribed to the SNS topic receive identical notifications for the new order\. An Amazon Elastic Compute Cloud \(Amazon EC2\) server instance attached to one of the SQS queues can handle the processing or fulfillment of the order\. And you can attach another Amazon EC2 server instance to a data warehouse for analysis of all orders received\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fanout.png)

You can also use fanout to replicate data sent to your production environment with your test environment\. Expanding upon the previous example, you can subscribe another SQS queue to the same SNS topic for new incoming orders\. Then, by attaching this new SQS queue to your test environment, you can continue to improve and test your application using data received from your production environment\.

**Important**  
Make sure that you consider data privacy and security before you send any production data to your test environment\.

For more information, see the following resources:
+ [Fanout to Amazon SQS queues](sns-sqs-as-subscriber.md)
+ [Fanout to AWS Lambda functions](sns-lambda-as-subscriber.md)
+ [Fanout to HTTP/S endpointsFanout to AWS Event Fork Pipelines](sns-http-https-endpoint-as-subscriber.md)
+ [ Event\-Driven Computing with Amazon SNS and AWS Compute, Storage, Database, and Networking Services](https://aws.amazon.com/blogs/compute/event-driven-computing-with-amazon-sns-compute-storage-database-and-networking-services/) 

### Application alerts<a name="SNSAlertsScenario"></a>

Application and system alerts are notifications that are triggered by predefined thresholds\. Amazon SNS can send these notifications to specified users via SMS and email\. For example, you can receive immediate notification when an event occurs, such as a specific change to your Amazon EC2 Auto Scaling group, a new file uploaded to an Amazon S3 bucket, or a metric threshold breached in Amazon CloudWatch\. For more information, see [Setting up Amazon SNS notifications](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_SetupSNS.html) in the *Amazon CloudWatch User Guide*\.

### User notifications<a name="SNSPushMessaging"></a>

Amazon SNS can send push email messages and text messages \(SMS messages\) to individuals or groups\. For example, you could send e\-commerce order confirmations as user notifications\. For more information about using Amazon SNS to send SMS messages, see [Mobile text messaging \(SMS\)](sns-mobile-phone-number-as-subscriber.md)\.

### Mobile push notifications<a name="SNSMobilePushScenario"></a>

Mobile push notifications enable you to send messages directly to mobile apps\. For example, you can use Amazon SNS to send update notifications to an app\. The notification message can include a link to download and install the update\. For more information about using Amazon SNS to send push notification messages, see [Mobile push notifications](sns-mobile-application-as-subscriber.md)\.