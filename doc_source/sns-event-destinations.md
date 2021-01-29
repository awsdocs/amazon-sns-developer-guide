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
|  Email  |  Deliver events to inboxes as email messages\. For more information, see [Email notifications](sns-email-notifications.md)\.  | 
|  SMS  |  Deliver events to mobile phones as text messages\. For more information, see [Mobile text messaging \(SMS\)](sns-mobile-phone-number-as-subscriber.md)\.  | 
|  Platform endpoint  |  Deliver events to mobile phones as native push notifications\. For more information, see [Mobile push notifications](sns-mobile-application-as-subscriber.md)\.  | 