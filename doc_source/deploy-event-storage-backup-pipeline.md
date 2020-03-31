# Tutorial: To deploy and subscribe the event storage and backup pipeline<a name="deploy-event-storage-backup-pipeline"></a>

This tutorial shows how to deploy the [Event Storage and Backup Pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-storage-and-backup-pipeline) and subscribe it to an Amazon SNS topic\. This process automatically turns the AWS SAM template associated with the pipeline into an AWS CloudFormation stack, and then deploys the stack into your AWS account\. This process also creates and configures the set of resources that comprise the Event Storage and Backup Pipeline, including the following:
+ Amazon SQS queue
+ Lambda function
+ Kinesis Data Firehose delivery stream
+ Amazon S3 backup bucket

For more information about configuring a stream with an S3 bucket as a destination, see `[S3DestinationConfiguration](https://docs.aws.amazon.com/firehose/latest/APIReference/API_S3DestinationConfiguration.html)` in the *Amazon Kinesis Data Firehose API Reference*\.

For more information about transforming events and about configuring event buffering, event compression, and event encryption, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

For more information about filtering events, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md) in this guide\.

1. Sign in to the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\.

1. On the navigation panel, choose **Functions** and then choose **Create function**\.

1. On the **Create function** page, do the following:

   1. Choose **Browse serverless app repository**, **Public applications**, **Show apps that create custom IAM roles or resource policies**\.

   1. Search for `fork-event-storage-backup-pipeline` and then choose the application\.

1. On the **fork\-event\-storage\-backup\-pipeline** page, do the following:

   1. In the **Application settings** section, enter an **Application name** \(for example, `my-app-backup`\)\.
**Note**  
For each deployment, the application name must be unique\. If you reuse an application name, the deployment will update only the previously deployed AWS CloudFormation stack \(rather than create a new one\)\.

   1. \(Optional\) For **BucketArn**, enter the ARN of the S3 bucket into which incoming events are loaded\. If you don't enter a value, a new S3 bucket is created in your AWS account\.

   1. \(Optional\) For **DataTransformationFunctionArn**, enter the ARN of the Lambda function through which the incoming events are transformed\. If you don't enter a value, data transformation is disabled\.

   1. \(Optional\) Enter one of the following **LogLevel** settings for the execution of your application's Lambda function:
      + `DEBUG`
      + `ERROR`
      + `INFO` \(default\)
      + `WARNING`

   1. For **TopicArn**, enter the ARN of the Amazon SNS topic to which this instance of the fork pipeline is to be subscribed\.

   1. \(Optional\) For **StreamBufferingIntervalInSeconds** and **StreamBufferingSizeInMBs**, enter the values for configuring the buffering of incoming events\. If you don't enter any values, 300 seconds and 5 MB are used\.

   1. \(Optional\) Enter one of the following **StreamCompressionFormat** settings for compressing incoming events:
      + `GZIP`
      + `SNAPPY`
      + `UNCOMPRESSED` \(default\)
      + `ZIP`

   1. \(Optional\) For **StreamPrefix**, enter the string prefix to name files stored in the S3 backup bucket\. If you don't enter a value, no prefix is used\.

   1. \(Optional\) For **SubscriptionFilterPolicy**, enter the Amazon SNS subscription filter policy, in JSON format, to be used for filtering incoming events\. The filter policy decides which events are stored in the S3 backup bucket\. If you don't enter a value, no filtering is used \(all events are stored\)\.

   1. Choose **I acknowledge that this app creates custom IAM roles, resource policies and deploys nested applications\.** and then choose **Deploy**\.

On the **Deployment status for *my\-app*** page, Lambda displays the **Your application is being deployed** status\.

In the **Resources** section, AWS CloudFormation begins to create the stack and displays the **CREATE\_IN\_PROGRESS** status for each resource\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.

When the deployment is complete, Lambda displays the **Your application has been deployed** status\.

Messages published to your Amazon SNS topic are stored in the S3 backup bucket provisioned by the Event Storage and Backup pipeline automatically\.