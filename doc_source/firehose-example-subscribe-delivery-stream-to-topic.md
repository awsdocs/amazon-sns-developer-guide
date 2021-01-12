# Subscribing the Kinesis Data Firehose delivery stream to the Amazon SNS topic<a name="firehose-example-subscribe-delivery-stream-to-topic"></a>

This page describes how to create the following for the [message archiving and analytics example use case](firehose-example-use-case.md):
+ The AWS Identity and Access Management \(IAM\) role that allows the Amazon SNS subscription to put records on the Amazon Kinesis Data Firehose delivery stream
+ The Kinesis Data Firehose delivery stream subscription to the SNS topic

**To create the IAM role for the Amazon SNS subscription**

1. Open the [Roles page](https://console.aws.amazon.com/iam/home?#/roles) of the IAM console\.

1. Choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\.

1. For **Choose a use case**, choose **SNS**\. Then choose **Next: Permissions**\.

1. Choose **Next: Tags**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter **ticketUploadStreamSubscriptionRole**\. Then choose **Create role**\.

1. When the role is created, choose its name \(**ticketUploadStreamSubscriptionRole**\)\.

1. On the role's **Summary** page, choose **Add inline policy**\.

1. On the **Create policy** page, choose the **JSON** tab, and then paste the following policy into the box:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Action": [
                   "firehose:DescribeDeliveryStream",
                   "firehose:ListDeliveryStreams",
                   "firehose:ListTagsForDeliveryStream",
                   "firehose:PutRecord",
                   "firehose:PutRecordBatch"
               ],
               "Resource": [
                   "arn:aws:firehose:us-east-1:123456789012:deliverystream/ticketUploadStream"
               ],
               "Effect": "Allow"
           }
       ]
   }
   ```

   In this policy, replace the AWS account number \(*123456789012*\) with your own, and change the AWS Region \(*us\-east\-1*\) accordingly\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter **FirehoseSnsPolicy**\. Then choose **Create policy**\.

1. On the role's **Summary** page, note the **Role ARN** for later\.

For more information on creating IAM roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

**To subscribe the Kinesis Data Firehose delivery stream to the SNS topic**

1. Open the [Topics page](https://console.aws.amazon.com/sns/home#/topics) of the Amazon SNS console\.

1. On the **Subscriptions**, tab, choose **Create subscription**\.

1. Under **Details**, for **Protocol**, choose **Amazon Kinesis Data Firehose**\.

1. For **Endpoint**, enter the Amazon Resource Name \(ARN\) of the **ticketUploadStream** delivery stream that you created earlier\. For example, enter **arn:aws:firehose:us\-east\-1:123456789012:deliverystream/ticketUploadStream**\.

1. For **Subscription role ARN**, enter the ARN of the **ticketUploadStreamSubscriptionRole** IAM role that you created earlier\. For example, enter **arn:aws:iam::123456789012:role/ticketUploadStreamSubscriptionRole**\.

1. Select the **Enable raw message delivery** check box\.

1. Choose **Create subscription**\.

You've created the IAM role and SNS topic subscription\. To continue, see [Testing and querying the configuration](firehose-example-test-and-query.md)\.