# Using Amazon SNS for system\-to\-system messaging with AWS Event Fork Pipelines as a subscriber<a name="sns-fork-pipeline-as-subscriber"></a>

You can use Amazon SNS to build event\-driven applications which use subscriber services to perform work automatically in response to events triggered by publisher services\. This architectural pattern can make services more reusable, interoperable, and scalable\. However, it can be labor\-intensive to fork the processing of events into pipelines that address common event handling requirements, such as event storage, backup, search, analytics, and replay\.

To accelerate the development of your event\-driven applications, you can subscribe event\-handling pipelines—powered by AWS Event Fork Pipelines—to Amazon SNS topics\. AWS Event Fork Pipelines is a suite of open\-source [nested applications](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-nested-applications.html), based on the [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/) \(AWS SAM\), which you can deploy directly from the [AWS Event Fork Pipelines suite](https://serverlessrepo.aws.amazon.com/applications?query=aws-event-fork-pipelines) \(choose **Show apps that create custom IAM roles or resource policies**\) into your AWS account\.

For an AWS Event Fork Pipelines use case, see [Tutorial: Deploying and testing the AWS Event Fork Pipelines sample application](sns-tutorial-deploy-test-fork-pipelines-sample-application.md)\.

**Topics**
+ [How AWS Event Fork Pipelines works](#how-sns-fork-works)
+ [Deploying AWS Event Fork Pipelines](#deploying-sns-fork-pipelines)

## How AWS Event Fork Pipelines works<a name="how-sns-fork-works"></a>

AWS Event Fork Pipelines is a serverless design pattern\. However, it is also a suite of nested serverless applications based on AWS SAM \(which you can deploy directly from the AWS Serverless Application Repository \(AWS SAR\) to your AWS account in order to enrich your event\-driven platforms\)\. You can deploy these nested applications individually, as your architecture requires\.

**Topics**
+ [The event storage and backup pipeline](#sns-fork-event-storage-and-backup-pipeline)
+ [The event search and analytics pipeline](#sns-fork-event-search-and-analytics-pipeline)
+ [The event replay pipeline](#sns-fork-event-replay-pipeline)

The following diagram shows an AWS Event Fork Pipelines application supplemented by three nested applications\. You can deploy any of the pipelines from the AWS Event Fork Pipelines suite on the AWS SAR independently, as your architecture requires\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fork-pipeline-as-subscriber-how-it-works.png)

Each pipeline is subscribed to the same Amazon SNS topic, allowing itself to process events in parallel as these events are published to the topic\. Each pipeline is independent and can set its own [Subscription Filter Policy](sns-subscription-filter-policies.md)\. This allows a pipeline to process only a subset of the events that it is interested in \(rather than all events published to the topic\)\.

**Note**  
Because you place the three AWS Event Fork Pipelines alongside your regular event processing pipelines \(possibly already subscribed to your Amazon SNS topic\), you don’t need to change any portion of your current message publisher to take advantage of AWS Event Fork Pipelines in your existing workloads\.

### The event storage and backup pipeline<a name="sns-fork-event-storage-and-backup-pipeline"></a>

The following diagram shows the [Event Storage and Backup Pipeline](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:077246666028:applications~fork-event-storage-backup-pipeline)\. You can subscribe this pipeline to your Amazon SNS topic to automatically back up the events flowing through your system\.

This pipeline is comprised of an Amazon SQS queue that buffers the events delivered by the Amazon SNS topic, an AWS Lambda function that automatically polls for these events in the queue and pushes them into an Amazon Kinesis Data Firehose stream, and an Amazon S3 bucket that durably backs up the events loaded by the stream\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fork-event-storage-and-backup-pipeline.png)

To fine\-tune the behavior of your Firehose stream, you can configure it to buffer, transform, and compress your events prior to loading them into the bucket\. As events are loaded, you can use Amazon Athena to query the bucket using standard SQL queries\. You can also configure the pipeline to reuse an existing Amazon S3 bucket or create a new one\.

### The event search and analytics pipeline<a name="sns-fork-event-search-and-analytics-pipeline"></a>

The following diagram shows the [Event Search and Analytics Pipeline](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:077246666028:applications~fork-event-search-analytics-pipeline)\. You can subscribe this pipeline to your Amazon SNS topic to index the events that flow through your system in a search domain and then run analytics on them\.

This pipeline is comprised of an Amazon SQS queue that buffers the events delivered by the Amazon SNS topic, an AWS Lambda function that polls events from the queue and pushes them into an Amazon Kinesis Data Firehose stream, an Amazon Elasticsearch Service domain that indexes the events loaded by the Firehose stream, and an Amazon S3 bucket that stores the dead\-letter events that can’t be indexed in the search domain\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fork-event-search-and-analytics-pipeline.png)

To fine\-tune your Firehose stream in terms of event buffering, transformation, and compression, you can configure this pipeline\.

You can also configure whether the pipeline should reuse an existing Elasticsearch domain in your AWS account or create a new one for you\. As events are indexed in the search domain, you can use Kibana to run analytics on your events and update visual dashboards in real\-time\. 

### The event replay pipeline<a name="sns-fork-event-replay-pipeline"></a>

The following diagram shows the [Event Replay Pipeline](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:077246666028:applications~fork-event-replay-pipeline)\. To record the events that have been processed by your system for the past 14 days \(for example when your platform needs to recover from failure\), you can subscribe this pipeline to your Amazon SNS topic and then reprocess the events\.

This pipeline is comprised of an Amazon SQS queue that buffers the events delivered by the Amazon SNS topic, and an AWS Lambda function that polls events from the queue and redrives them into your regular event processing pipeline, which is also subscribed to your topic\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fork-event-replay-pipeline.png)

**Note**  
By default, the replay function is disabled, not redriving your events\. If you need to reprocess events, you must enable the Amazon SQS replay queue as an event source for the AWS Lambda replay function\.

## Deploying AWS Event Fork Pipelines<a name="deploying-sns-fork-pipelines"></a>

The [AWS Event Fork Pipelines suite](https://serverlessrepo.aws.amazon.com/applications?query=aws-event-fork-pipelines) \(choose **Show apps that create custom IAM roles or resource policies**\) is available as a group of public applications in the AWS Serverless Application Repository, from where you can deploy and test them manually using the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\. For information about deploying pipelines using the AWS Lambda console, see [Subscribing AWS Event Fork Pipelines to an Amazon SNS topic](sns-tutorial-subscribe-event-fork-pipelines.md)\.

In a production scenario, we recommend embedding AWS Event Fork Pipelines within your overall application's AWS SAM template\. The nested\-application feature lets you do this by adding the resource `[AWS::Serverless::Application](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template.html#serverless-sam-template-application)` to your AWS SAM template, referencing the AWS SAR `ApplicationId` and the `SemanticVersion` of the nested application\.

For example, you can use the Event Storage and Backup Pipeline as a nested application by adding the following YAML snippet to the `Resources` section of your AWS SAM template\.

```
Backup:
  Type: AWS::Serverless::Application
  Properties:
    Location:
      ApplicationId: arn:aws:serverlessrepo:us-east-2:123456789012:applications/fork-event-storage-backup-pipeline
      SemanticVersion: 1.0.0
    Parameters: 
      #The ARN of the Amazon SNS topic whose messages should be backed up to the Amazon S3 bucket.
      TopicArn: !Ref MySNSTopic
```

When you specify parameter values, you can use AWS CloudFormation intrinsic functions to reference other resources in your template\. For example, in the YAML snippet above, the `TopicArn` parameter references the `[AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html)` resource `MySNSTopic`, defined elsewhere in the AWS SAM template\. For more information, see the [Intrinsic Function Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html) in the *AWS CloudFormation User Guide*\.

**Note**  
The AWS Lambda console page for your AWS SAR application includes the **Copy as SAM Resource** button, which copies the YAML required for nesting an AWS SAR application to the clipboard\.