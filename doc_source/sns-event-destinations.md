# Amazon SNS event destinations<a name="sns-event-destinations"></a>

This page lists all destinations that can receive information on events, grouped by [application\-to\-application \(A2A\) messaging](sns-system-to-system-messaging.md) and [application\-to\-person \(A2P\) notifications](sns-user-notifications.md)\.

**Note**  
Amazon SNS introduced [FIFO topics](sns-fifo-topics.md) in October, 2020\. Currently, most AWS services support receiving events from SNS standard topics only\. Amazon SQS supports receiving events from both SNS standard and FIFO topics\.

## A2A destinations<a name="sns-event-destinations-a2a"></a>


| Event destination | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)  |  Deliver events to delivery streams for archiving and analysis purposes\. Through delivery streams, you can deliver events to AWS destinations like Amazon Simple Storage Service \(Amazon S3\), Amazon Redshift, and Amazon Elasticsearch Service \(Amazon ES\), or to third\-party destinations such as Datadog, New Relic, MongoDB, and Splunk\. For more information, see [Fanout to Kinesis Data Firehose delivery streams](sns-firehose-as-subscriber.md)\.  | 
|  [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)  |  Deliver events to functions for triggering the execution of custom business logic\. For more information, see [Fanout to Lambda functions](sns-lambda-as-subscriber.md)\.  | 
|  [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)  |  Deliver events to queues for application integration purposes\. For more information, see [Fanout to Amazon SQS queues](sns-sqs-as-subscriber.md)\.  | 
|  AWS Event Fork Pipelines  |  Deliver events to event backup and storage, event search and analytics, or event replay pipelines\. For more information, see [Fanout to AWS Event Fork Pipelines](sns-fork-pipeline-as-subscriber.md)\.  | 
|  HTTP/S  |  Deliver events to external webhooks\. For more information, see [Fanout to HTTP/S endpoints](sns-http-https-endpoint-as-subscriber.md)\.  | 

## A2P destinations<a name="sns-event-destinations-a2p"></a>


| Event destination | Benefit of using with Amazon SNS | 
| --- | --- | 
|  SMS  |  Deliver events to mobile phones as text messages\. For more information, see [Mobile text messaging \(SMS\)](sns-mobile-phone-number-as-subscriber.md)\.  | 
|  Email  |  Deliver events to inboxes as email messages\. For more information, see [Email notifications](sns-email-notifications.md)\.  | 
|  Platform endpoint  |  Deliver events to mobile phones as native push notifications\. For more information, see [Mobile push notifications](sns-mobile-application-as-subscriber.md)\.  | 
|  [AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/what-is.html)  |  Deliver events to Amazon Chime chat rooms or Slack channels\. For more information, see the following pages in the *AWS Chatbot Administrator Guide*: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-event-destinations.html)  | 
|  PagerDuty  |  Deliver operational insights to on\-call teams\. For more information, see [Deliver ML\-powered operational insights to your on\-call teams via PagerDuty with Amazon DevOps Guru](https://aws.amazon.com/blogs/mt/deliver-ml-powered-operational-insights-to-your-on-call-teams-via-pagerduty-with-amazon-devops-guru/) on the *AWS Management & Governance Blog*\.  | 

**Note**  
You can deliver both native AWS events and custom events to chat apps:  
**Native AWS events** – You can use AWS Chatbot to send native AWS events, through Amazon SNS topics, to Amazon Chime and Slack\. The supported set of native AWS events includes events from AWS Billing and Cost Management, AWS Health, AWS CloudFormation, Amazon CloudWatch, and more\. For more information, see [Using AWS Chatbot with other services](https://docs.aws.amazon.com/chatbot/latest/adminguide/related-services.html) in the *AWS Chatbot Administrator Guide*\.
**Custom events** – You can also send your custom events, through Amazon SNS topics, to Amazon Chime, Slack, and Microsoft Teams\. To do this, you publish custom events to an SNS topic, which delivers the events to a subscribed Lambda function\. The Lambda function then uses the chat app's webhook to deliver the events to recipients\. For more information, see [How do I use webhooks to publish Amazon SNS messages to Amazon Chime, Slack, or Microsoft Teams?](https://aws.amazon.com/premiumsupport/knowledge-center/sns-lambda-webhooks-chime-slack-teams/)