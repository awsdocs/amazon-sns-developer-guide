# Tutorial: To deploy and subscribe the event search and analytics pipeline<a name="deploy-event-search-analytics-pipeline"></a>

This tutorial shows how to deploy the [Event Search and Analytics Pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-search-and-analytics-pipeline) and subscribe it to an Amazon SNS topic\. This process automatically turns the AWS SAM template associated with the pipeline into an AWS CloudFormation stack, and then deploys the stack into your AWS account\. This process also creates and configures the set of resources that comprise the Event Search and Analytics Pipeline, including the following:
+ Amazon SQS queue
+ Lambda function
+ Kinesis Data Firehose delivery stream
+ Amazon Elasticsearch Service domain
+ Amazon S3 dead\-letter bucket

For more information about configuring a stream with an index as a destination, see `[ElasticsearchDestinationConfiguration](https://docs.aws.amazon.com/firehose/latest/APIReference/API_ElasticsearchDestinationConfiguration.html)` in the *Amazon Kinesis Data Firehose API Reference*\.

For more information about transforming events and about configuring event buffering, event compression, and event encryption, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

For more information about filtering events, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md) in this guide\.

1. Sign in to the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\.

1. On the navigation panel, choose **Functions** and then choose **Create function**\.

1. On the **Create function** page, do the following:

   1. Choose **Browse serverless app repository**, **Public applications**, **Show apps that create custom IAM roles or resource policies**\.

   1. Search for `fork-event-search-analytics-pipeline` and then choose the application\.

1. On the **fork\-event\-search\-analytics\-pipeline** page, do the following:

   1. In the **Application settings** section, enter an **Application name** \(for example, `my-app-search`\)\.
**Note**  
For each deployment, the application name must be unique\. If you reuse an application name, the deployment will update only the previously deployed AWS CloudFormation stack \(rather than create a new one\)\.

   1. \(Optional\) For **DataTransformationFunctionArn**, enter the ARN of the Lambda function used for transforming incoming events\. If you don't enter a value, data transformation is disabled\.

   1. \(Optional\) Enter one of the following **LogLevel** settings for the execution of your application's Lambda function:
      + `DEBUG`
      + `ERROR`
      + `INFO` \(default\)
      + `WARNING`

   1. \(Optional\) For **SearchDomainArn**, enter the ARN of the Amazon ES domain, a cluster that configures the needed compute and storage functionality\. If you don't enter a value, a new domain is created with the default configuration\.

   1. For **TopicArn**, enter the ARN of the Amazon SNS topic to which this instance of the fork pipeline is to be subscribed\.

   1. For **SearchIndexName**, enter the name of the Amazon ES index for event search and analytics\.
**Note**  
The following quotas apply to index names:  
Can't include uppercase letters
Can't include the following characters: `\ / * ? " < > | ` , #`
Can't begin with the following characters: `- + _`
Can't be the following: `. ..`
Can't be longer than 80 characters
Can't be longer than 255 bytes
Can't contain a colon \(from Amazon ES 7\.0\)

   1. \(Optional\) Enter one of the following **SearchIndexRotationPeriod** settings for the rotation period of the Amazon ES index:
      + `NoRotation` \(default\)
      + `OneDay`
      + `OneHour`
      + `OneMonth`
      + `OneWeek`

      Index rotation appends a timestamp to the index name, facilitating the expiration of old data\. 

   1. For **SearchTypeName**, enter the name of the Amazon ES type for organizing the events in an index\.
**Note**  
Amazon ES type names can contain any character \(except null bytes\) but can't begin with `_`\.
For Amazon ES 6\.x, there can be only one type per index\. If you specify a new type for an existing index that already has another type, Kinesis Data Firehose returns a runtime error\.

   1. \(Optional\) For **StreamBufferingIntervalInSeconds** and **StreamBufferingSizeInMBs**, enter the values for configuring the buffering of incoming events\. If you don't enter any values, 300 seconds and 5 MB are used\.

   1. \(Optional\) Enter one of the following **StreamCompressionFormat** settings for compressing incoming events:
      + `GZIP`
      + `SNAPPY`
      + `UNCOMPRESSED` \(default\)
      + `ZIP`

   1. \(Optional\) For **StreamPrefix**, enter the string prefix to name files stored in the S3 dead\-letter bucket\. If you don't enter a value, no prefix is used\.

   1. \(Optional\) For **StreamRetryDurationInSecons**, enter the retry duration for cases when Kinesis Data Firehose can't index events in the Amazon ES index\. If you don't enter a value, then 300 seconds is used\.

   1. \(Optional\) For **SubscriptionFilterPolicy**, enter the Amazon SNS subscription filter policy, in JSON format, to be used for filtering incoming events\. The filter policy decides which events are indexed in the Amazon ES index\. If you don't enter a value, no filtering is used \(all events are indexed\)\.

   1. Choose **I acknowledge that this app creates custom IAM roles, resource policies and deploys nested applications\.** and then choose **Deploy**\.

On the **Deployment status for *my\-app\-search*** page, Lambda displays the **Your application is being deployed** status\.

In the **Resources** section, AWS CloudFormation begins to create the stack and displays the **CREATE\_IN\_PROGRESS** status for each resource\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.

When the deployment is complete, Lambda displays the **Your application has been deployed** status\.

Messages published to your Amazon SNS topic are indexed in the Amazon ES index provisioned by the Event Search and Analytics pipeline automatically\. If the pipeline can't index an event, it stores it in a S3 dead\-letter bucket\.