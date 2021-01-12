# Creating the Kinesis Data Firehose delivery stream<a name="firehose-example-create-delivery-stream"></a>

This page describes how to create the Amazon Kinesis Data Firehose delivery stream for the [message archiving and analytics example use case](firehose-example-use-case.md)\.

**To create the Kinesis Data Firehose delivery stream**

1. Open the [Amazon Kinesis services console](https://console.aws.amazon.com/kinesis/home)\.

1. Choose **Kinesis Data Firehose** and then choose **Create delivery stream**\.

1. On the **New delivery stream** page, for **Delivery stream name**, enter **ticketUploadStream**, and then choose **Next**\.

1. On the **Process records** page, choose **Next**\.

1. On the **Choose a destination** page, do the following:

   1. For **Destination**, choose **Amazon S3**\.

   1. Under **S3 destination**, for **S3 bucket**, choose the S3 bucket that you [created initially](firehose-example-initial-resources.md)\.

   1. Choose **Next**\.

1. On the **Configure settings** page, for **S3 buffer conditions**, do the following:
   + For **Buffer size**, enter **1**\.
   + For **Buffer interval**, enter **60**\.

   Using these values for the Amazon S3 buffer lets you quickly test the configuration\. The first condition that is satisfied triggers data delivery to the S3 bucket\.

1. On the **Configure settings** page, for **Permissions**, choose to create an AWS Identity and Access Management \(IAM\) role with the required permissions assigned automatically\. Then choose **Next**\.

1. On the **Review** page, choose **Create delivery stream**\.

1. From the **Kinesis Data Firehose delivery streams page,** choose the delivery stream you just created \(**ticketUploadStream**\)\. On the **Details** tab, note the stream's Amazon Resource Name \(ARN\) for later\.

For more information on creating delivery streams, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) in the *Amazon Kinesis Data Firehose Developer Guide*\. For more information on creating IAM roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

You've created the Kinesis Data Firehose delivery stream with the required permissions\. To continue, see [Subscribing the Kinesis Data Firehose delivery stream to the Amazon SNS topic](firehose-example-subscribe-delivery-stream-to-topic.md)\.