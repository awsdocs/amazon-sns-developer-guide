# Data protection policy operations<a name="sns-message-data-protection-operations"></a>

The following are examples of data protection policies that you can use to audit and deny sensitive data\. For a complete tutorial that includes an example application, see the [Introducing message data protection for Amazon SNS](https://aws.amazon.com/blogs/compute/introducing-message-data-protection-for-amazon-sns/) blog post\.

**Topics**
+ [Audit operation](#statement-operation-json-properties-audit)
+ [De\-identify operation](#statement-operation-json-properties-deidentify)
+ [Deny operation](#statement-operation-json-properties-deny)

## Audit operation<a name="statement-operation-json-properties-audit"></a>

The **Audit** operation samples topic inbound messages, and logs the sensitive data findings in an AWS destination\. The sample rate can be an integer between 0–99\. This operation requires one of the following types of logging destinations:

1. **FindingsDestination** – The logging destination when the Amazon SNS topic finds sensitive data in the payload\.

1. **NoFindingsDestination** – The logging destination when the Amazon SNS topic doesn't find sensitive data in the payload\.

You can use the following AWS services in each of the following log destination types:
+ **Amazon CloudWatch Logs** \(Optional\) – The `LogGroup` must be in the topic region and the name must start with **/aws/vendedlogs/**\.
+ **Amazon Kinesis Data Firehose **\(Optional\) – The `DeliveryStream` must be in the topic region and have **Direct PUT** as the source of delivery stream\. For additional details, see [Source, Destination, and Name](https://docs.aws.amazon.com/firehose/latest/dev/create-name.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.
+ **Amazon S3** \(Optional\) – An Amazon S3 bucket name\.

```
{
  "Operation": {
    "Audit": {
      "SampleRate": "99",
      "FindingsDestination": {
            "CloudWatchLogs": {
                "LogGroup": "/aws/vendedlogs/log-group-name"
            },
            "Firehose": {
                "DeliveryStream": "delivery-stream-name"
            },
            "S3": {
                "Bucket": "bucket-name"
            }
      },
      "NoFindingsDestination": {
            "CloudWatchLogs": {
                "LogGroup": "/aws/vendedlogs/log-group-name"
            },
            "Firehose": {
                "DeliveryStream": "delivery-stream-name"
            },
            "S3": {
                "Bucket": "bucket-name"
            }
      }
    }
  }
}
```

### Required permissions when specifying log destinations<a name="required-permissions-log-operations"></a>

When you specify logging destinations in the data protection policy, you must add the following permissions to the IAM identity policy of the IAM principal that is calling the Amazon SNS `PutDataProtectionPolicy` API, or the `CreateTopic` API with the `--data-protection-policy` parameter\.


| Audit destination | IAM permission | 
| --- | --- | 
| Default | logs:CreateLogDelivery logs:GetLogDelivery logs:UpdateLogDelivery logs:DeleteLogDelivery logs:ListLogDeliveries  | 
| CloudWatchLogs | logs:PutResourcePolicy logs:DescribeResourcePolicies logs:DescribeLogGroups  | 
| Firehose | iam:CreateServiceLinkedRole firehose:TagDeliveryStream  | 
| S3 | s3:PutBucketPolicy s3:GetBucketPolicy  | 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogDelivery",
        "logs:GetLogDelivery",
        "logs:UpdateLogDelivery",
        "logs:DeleteLogDelivery",
        "logs:ListLogDeliveries"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "logs:PutResourcePolicy",
        "logs:DescribeResourcePolicies",
        "logs:DescribeLogGroups"
      ],
      "Resource": [
        "arn:aws:logs:region:account-id:SampleLogGroupName:*:*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceLinkedRole",
        "firehose:TagDeliveryStream"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutBucketPolicy",
        "s3:GetBucketPolicy"
      ],
      "Resource": [
        "arn:aws:s3:::bucket-name"
      ]
    }
  ]
}
```

### Audit destination log example<a name="data-protection-policy-audit-destination-log"></a>

You can use `callerPrincipal` to identify the source of the sensitive content, and use the `messageID` as a reference to check against the `Publish` API response\.

```
{
  "messageId": "34d9b400-c6dd-5444-820d-fbeb0f1f54cf",
  "auditTimestamp": "2022-05-12T2:10:44Z",
  "callerPrincipal": "arn:aws:iam::123412341234:role/Publisher",
  "resourceArn": "arn:aws:sns:us-east-1:123412341234:PII-data-topic",
  "dataIdentifiers": [
    {
      "name": "Name",
      "count": 1,
      "detections": [
        {
          "start": 1,
          "end": 2
        }
      ]
    },
    {
      "name": "PhoneNumber",
      "count": 2,
      "detections": [
        {
          "start": 3,
          "end": 4
        },
        {
          "start": 5,
          "end": 6
        }
      ]
    }
  ]
}
```

### Audit operation metrics<a name="data-protection-policy-audit-metrics"></a>

When an audit operation has specified the `FindingsDestination` or the `NoFindingsDestination` property, the topic owners also receive CloudWatch `MessagesWithFindings` and `MessagesWithNoFindings` metrics\.

![\[Example of an audit displaying data over a specified period of time.\]](http://docs.aws.amazon.com/sns/latest/dg/images/audit-operations-metrics.png)

## De\-identify operation<a name="statement-operation-json-properties-deidentify"></a>

The **De\-identify** operation masks or redacts sensitive data from published messages\. This operation is available for inbound messages only, and requires one of the following types of configurations:
+ **MaskConfig** – Mask using a supported character from the following table\. For example, ssn: `123-45-6789` becomes ssn: `###########`\.

  ```
  {
  "Operation": {
      "Deidentify": {
          "MaskConfig": {
              "MaskWithCharacter": "#"
            }
      }
  }
  ```    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-message-data-protection-operations.html)
+ **RedactConfig** – Redact by removing the data entirely\. For example, ssn: `123-45-6789` becomes ssn:` `\.

  ```
  {
  "Operation": {
      "Deidentify": {
          "RedactConfig": {}
      }
  }
  ```

On an inbound message, the sensitive data is de\-identified after the audit operation, and the `SNS:Publish` API caller receives the following invalid parameter error when the entire message is sensitive\.

`Error code: AuthorizationError ...`

## Deny operation<a name="statement-operation-json-properties-deny"></a>

The **Deny** operation interrupts either the `Publish` API request or the delivery of the message if the message contains sensitive data\. The Deny operation object is empty, as it doesn't require additional configuration\.

```
"Operation": {
    "Deny": {}
}
```

On an inbound message, the `SNS:Publish` API caller receives an authorization error\.

`Error code: AuthorizationError ...`

On an outbound message, the Amazon SNS topic does not deliver the message to the subscription\. To track unauthorized deliveries, enable the topic’s [delivery status logging](sns-topic-attributes.md)\. The following is an example of a delivery status log:

```
{
    "notification": {
        "messageMD5Sum": "29638742ffb68b32cf56f42a79bcf16b",
        "messageId": "34d9b400-c6dd-5444-820d-fbeb0f1f54cf",
        "topicArn": "arn:aws:sns:us-east-1:123412341234:PII-data-topic",
        "timestamp": "2022-05-12T2:12:44Z"
    },
    "delivery": {
        "deliveryId": "98236591c-56aa-51ee-a5ed-0c7d43493170",
        "destination": "arn:aws:sqs:us-east-1:123456789012:NoNameAccess",
        "providerResponse": "The topic's data protection policy prohibits this message from being delivered to <subscription-arn>",
        "dwellTimeMs":20,
        "attempts":1,
        "statusCode": 403
    },
    "status": "FAILURE"
}
```