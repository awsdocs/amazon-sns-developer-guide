# Data protection policy examples<a name="sns-message-data-protection-examples"></a>

The following are examples of data protection policies that you can use to audit and deny sensitive data\. For a complete tutorial that includes an example application, see the [Introducing message data protection for Amazon SNS](https://aws.amazon.com/blogs/compute/introducing-message-data-protection-for-amazon-sns/) blog post\.

**Topics**
+ [Example policy for auditing](#sns-message-data-protection-audit-example)
+ [Example policy with inbound de\-identify statement](#sns-message-data-protection-inbound-deidentify-example)
+ [Example policy with inbound deny statement](#sns-message-data-protection-inbound-deny-example)
+ [Example policy with outbound deny statement](#sns-message-data-protection-outbound-deny-example)

## Example policy for auditing<a name="sns-message-data-protection-audit-example"></a>

Audit policies allow you to audit up to 99% of inbound messages and send findings to [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html), and [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)\.

For example, you can create an audit policy to evaluate whether any of your systems are inadvertently sending or receiving sensitive data\. If your audit results show that systems are sending credit card information to systems that don’t require it, you can implement a data protection policy to block the delivery of the data\.

The following example audits 99% of the messages that flow through the topic by looking for credit card numbers and sending the findings to CloudWatch Logs , Kinesis Data Firehose, and Amazon S3\.

**Data protection policy**:

```
{
  "Name": "__example_data_protection_policy",
  "Description": "Example data protection policy",
  "Version": "2021-06-01",
  "Statement": [
    {
      "DataDirection": "Inbound",
      "Principal": ["*"],
      "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber"
      ],
      "Operation": {
        "Audit": {
          "SampleRate": "99",
          "FindingsDestination": {
            "CloudWatchLogs": {
              "LogGroup": "<example log name>"
            },
            "Firehose": {
              "DeliveryStream": "<example stream name>"
            },
            "S3": {
              "Bucket": "<example bucket name>"
            }
          }
        }
      }
    }
  ]
}
```

**Audit results format example**:

```
{
    "messageId": "...",
    "callerPrincipal": "arn:aws:sts::123456789012:assumed-role/ExampleRole",
    "resourceArn": "arn:aws:sns:us-east-1:123456789012:ExampleArn", 
    "dataIdentifiers": [
        {
            "name": "CreditCardNumber",
            "count": 1,
            "detections": [
                { "start": 1, "end": 2 }
            ]
        }
    ],
    "timestamp": "2021-04-20T00:33:40.241Z"
}
```

## Example policy with inbound de\-identify statement<a name="sns-message-data-protection-inbound-deidentify-example"></a>

The following example prevents a user from publishing a message to a topic with `CreditCardNumber` by redacting the sensitive data from the message content\.

```
{
   "Name": "__example_data_protection_policy",
   "Description": "Example data protection policy",
   "Version": "2021-06-01",
   "Statement": [
      {
         "DataDirection": "Inbound",
         "Principal": [
            "arn:aws:iam::123456789012:user/ExampleUser"
         ],
         "DataIdentifier": [
            "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber"
         ],
         "Operation": {
            "Deidentify": {
               "RedactConfig": {}
            }
         }
      }
   ]
}
```

**Inbound de\-identify redact results example:**

```
// published message
My credit card number is 4539894458086459

// delivered message
My credit card number is
```

## Example policy with inbound deny statement<a name="sns-message-data-protection-inbound-deny-example"></a>

The following example blocks a user from publishing a message to a topic with `CreditCardNumber` in the message content\. Denied payloads in the API response have a status code of "403 AuthorizationError"\.

```
{
  "Name": "__example_data_protection_policy",
  "Description": "Example data protection policy",
  "Version": "2021-06-01",
  "Statement": [
    {
      "DataDirection": "Inbound",
      "Principal": [
        "arn:aws:iam::123456789012:user/ExampleUser"
      ],
      "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber"
      ],
      "Operation": {
        "Deny": {}
      }
    }
  ]
}
```

## Example policy with outbound deny statement<a name="sns-message-data-protection-outbound-deny-example"></a>

The following example blocks an AWS account from receiving messages that contain `CreditCardNumber`\.

```
{
  "Name": "__example_data_protection_policy",
  "Description": "Example data protection policy",
  "Version": "2021-06-01",
  "Statement": [
    {
      "DataDirection": "Outbound",
      "Principal": [
        "arn:aws:iam::123456789012:user/ExampleUser"
      ],
      "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/CreditCardNumber"
      ],
      "Operation": {
        "Deny": {}
      }
    }
  ]
}
```

**Outbound deny results example, logged in Amazon CloudWatch:**

```
{
  "notification": {
    "messageMD5Sum": "2e8f58ff2eeed723b56b15493fbfb5a5",
    "messageId": "8747a956-ebf1-59da-b291-f2c2e4b87c9c",
    "topicArn": "arn:aws:sns:us-east-2:664555388960:test1",
    "timestamp": "2022-09-08 15:40:57.144"
  },
  "delivery": {
    "deliveryId": "6a422437-78cc-5171-ad64-7fa3778507aa",
    "destination": "arn:aws:sqs:us-east-2:664555388960:test",
    "providerResponse": "The topic's data protection policy prohibits this message from being delivered to <subscription arn>",
    "dwellTimeMs": 22,
    "attempts": 1,
    "statusCode": 403
  },
  "status": "FAILURE"
}
```