# Prerequisites for subscribing Kinesis Data Firehose delivery streams to Amazon SNS topics<a name="prereqs-kinesis-data-firehose"></a>

To subscribe an Amazon Kinesis Data Firehose delivery stream to an SNS topic, your AWS account must have:
+ A standard SNS topic\. For more information, see [Creating an Amazon SNS topic](sns-create-topic.md)\.
+ A Kinesis Data Firehose delivery stream\. For more information, see [Creating an Amazon Kinesis Data Firehose Delivery Stream](https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html) and [Grant Your Application Access to Your Kinesis Data Firehose Resources](https://docs.aws.amazon.com/firehose/latest/dev/controlling-access.html#access-to-firehose) in the *Amazon Kinesis Data Firehose Developer Guide*\.
+ An AWS Identity and Access Management \(IAM\) role that trusts the Amazon SNS service principal and has permission to write to the delivery stream\. You'll enter this role's Amazon Resource Name \(ARN\) as the `SubscriptionRoleARN` when you create the subscription\. Amazon SNS assumes this role, which allows Amazon SNS to put records in the Kinesis Data Firehose delivery stream\.

  The following example policy shows the recommended permissions:

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
          "arn:aws:firehose:us-east-1:111111111111:deliverystream/firehose-sns-delivery-stream"
        ],
        "Effect": "Allow"
      }
    ]
  }
  ```

  To provide full permission for using Kinesis Data Firehose, you can also use the AWS managed policy `AmazonKinesisFirehoseFullAccess`\. Or, to provide stricter permissions for using Kinesis Data Firehose, you can create your own policy\. At minimum, the policy must provide permission to run the `PutRecord` operation on a specific delivery stream\.

  In all cases, you must also edit the trust relationship to include the Amazon SNS service principal\. For example:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {
          "Service": "sns.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  }
  ```

  For more information on creating roles, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

After you've completed these requirements, you can [subscribe the delivery stream to the SNS topic](firehose-endpoints-subscribe.md)\.