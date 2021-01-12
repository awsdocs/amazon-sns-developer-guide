# Creating the initial resources<a name="firehose-example-initial-resources"></a>

This page describes how to create the following resources for the [message archiving and analytics example use case](firehose-example-use-case.md):
+ An Amazon Simple Storage Service \(Amazon S3\) bucket
+ Two Amazon Simple Queue Service \(Amazon SQS\) queues
+ An Amazon SNS topic
+ Two Amazon SQS subscriptions to the Amazon SNS topic

**To create the initial resources**

1. Create the Amazon S3 bucket:<a name="firehose-use-case-create-bucket"></a>

   1. Open the [Amazon S3 console](https://console.aws.amazon.com/s3/home)\.

   1. Choose **Create bucket**\. 

   1. For **Bucket name**, enter a globally unique name\. Keep the other fields as the defaults\.

   1. Choose **Create bucket**\.

   For more information about Amazon S3 buckets, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide* and [Working with Amazon S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html) in the *Amazon Simple Storage Service Developer Guide*\.

1. Create the two Amazon SQS queues:

   1. Open the [Amazon SQS console](https://console.aws.amazon.com/sqs/home)\.

   1. Choose **Create queue**\.

   1. For **Type**, choose **Standard**\.

   1. For **Name**, enter **ticketPaymentQueue**\.

   1. Under **Access policy**, for **Choose method**, choose **Advanced**\.

   1. In the JSON policy box, paste the following policy:

      ```
      {
        "Version": "2008-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Principal": {
              "Service": "sns.amazonaws.com"
            },
            "Action": "sqs:SendMessage",
            "Resource": "*",
            "Condition": {
              "ArnEquals": {
                "aws:SourceArn": "arn:aws:sns:us-east-1:123456789012:ticketTopic"
              }
            }
          }
        ]
      }
      ```

      In this access policy, replace the AWS account number \(*123456789012*\) with your own, and change the AWS Region \(*us\-east\-1*\) accordingly\.

   1. Choose **Create queue**\.

   1. Repeat these steps to create a second SQS queue named **ticketFraudQueue**\.

   For more information on creating SQS queues, see [Creating an Amazon SQS queue \(console\)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-create-queue.html) in the *Amazon Simple Queue Service Developer Guide*\.

1. Create the SNS topic:

   1. Open the [Topics page](https://console.aws.amazon.com/sns/home#/topics) of the Amazon SNS console\.

   1. Choose **Create topic**\.

   1. Under **Details**, for **Type**, choose **Standard**\.

   1. For **Name**, enter **ticketTopic**\.

   1. Choose **Create topic**\.

   For more information on creating SNS topics, see [Creating an Amazon SNS topic](sns-create-topic.md)\.

1. Subscribe both SQS queues to the SNS topic:

   1. In the [Amazon SNS console](https://console.aws.amazon.com/sns/home#/topics), on the **ticketTopic** topic's details page, choose **Create subscription**\.

   1. Under **Details**, for **Protocol**, choose **Amazon SQS**\.

   1. For **Endpoint**, choose the Amazon Resource Name \(ARN\) of the **ticketPaymentQueue** queue\.

   1. Choose **Create subscription**\.

   1. Repeat these steps to create a second subscription using the ARN of the **ticketFraudQueue** queue\.

      For more information on subscribing to SNS topics, see [Subscribing to an Amazon SNS topic](sns-create-subscribe-endpoint-to-topic.md)\. You can also subscribe SQS queues to SNS topics from the Amazon SQS console\. For more information, see [Subscribing an Amazon SQS queue to an Amazon SNS topic \(console\)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-subscribe-queue-sns-topic.html) in the *Amazon Simple Queue Service Developer Guide*\.

You've created the initial resources for this example use case\. To continue, see [Creating the Kinesis Data Firehose delivery stream](firehose-example-create-delivery-stream.md)\.