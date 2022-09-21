# Understanding data protection policies<a name="sns-message-data-protection-policies"></a>

**Topics**
+ [What are data protection policies?](#what-are-data-protection-policies)
+ [How is the data protection policy structured?](#overview-of-data-protection-policies)
+ [How do I determine the IAM principals for my data protection policy?](#data-protection-policy-iam-principal-determined)
+ [Creating data protection policies to secure message data in Amazon SNS](sns-message-data-protection-configure.md)
+ [Deleting data protection policies in Amazon SNS](sns-message-data-protection-delete.md)
+ [Data protection policy examples](sns-message-data-protection-examples.md)

## What are data protection policies?<a name="what-are-data-protection-policies"></a>

Amazon SNS uses **data protection policies** to select the sensitive data for which you want to scan, and the actions that you want to take to protect that data from being exchanged by your Amazon SNS topics\. To select the sensitive data of interest, you use [data identifiers](sns-message-data-protection-data-identifiers.md)\. Amazon SNS message data protection then detects the sensitive data by using machine learning and pattern matching\. To act upon data identifiers that are found, you can define an **audit** or **deny** operation\. These operations let you log the sensitive data that is found \(or not found\), or deny message delivery\.

![\[Message data protection uses data protection policies to monitor sensitive data going into and out of your Amazon SNS topic.\]](http://docs.aws.amazon.com/sns/latest/dg/images/message-data-protection-policies-overview.png)

## How is the data protection policy structured?<a name="overview-of-data-protection-policies"></a>

As illustrated in the following figure, a data protection policy document includes these elements:
+ Optional policy\-wide information at the top of the document
+ One or more individual statements

Each statement includes information about a single permission\.

![\[Statement to policy statement example\]](http://docs.aws.amazon.com/sns/latest/dg/images/payload-policy-process.png)

Only one data protection policy can be defined per Amazon SNS topic\. The data protection policy can have one or more deny statements, and one audit statement\.

### JSON properties for the data protection policy<a name="data-protection-policy-json-properties"></a>

A data protection policy requires the following basic policy information for identification:
+ **Name** – The policy name\.
+ **Description** \(Optional\) – The policy description\.
+ **Version** – The policy language version\. The current version is 2021\-06\-01\.
+ **Statement** – A list of statements that specifies data protection policy actions\.

```
{
  "Name": "basicPII-protection",
  "Description": "Protect basic types of sensitive data",
  "Version": "2021-06-01",
  "Statement": [
        ...
  ]
}
```

### JSON properties for a policy statement<a name="policy-statement-json-properties"></a>

A policy statement sets the detection context for the data protection operation\.
+ **Sid** \(Optional\) – The statement identifier\.
+ **DataDirection** – Inbound \(for Publish API requests\) or Outbound \(for notification deliveries\) with respect to the Amazon SNS topic\.
+ **DataIdentifier** – The sensitive data for which the Amazon SNS topic should scan\. For example, name, address, or phone number\.
+ **Principal** – The IAM principal that published to the topic, or the IAM principal that subscribed to the topic\.
+ **Operation** – The follow\-on action, either **Deny** \(block\) or **Audit**, which the Amazon SNS topic executes once it finds sensitive data\.

```
{
    "Sid": "basicPII-inbound-protection",
    "DataDirection": "Inbound",
    "Principal": ["*"],
    "DataIdentifier": [
        "arn:aws:dataprotection::aws:data-identifier/Name",
        "arn:aws:dataprotection::aws:data-identifier/PhoneNumber-US"
    ],
    "Operation": {
        ...
    }
}
```

### JSON properties for a policy statement operation<a name="statement-operation-json-properties"></a>

A policy statement sets one of the following data protection operations\.
+ **Deny** – Blocks the Amazon SNS publish request or fails the message delivery\.
+ **Audit** – Emits metrics and finding logs without interrupting message publishing or delivery\.

#### Deny operation<a name="statement-operation-json-properties-deny"></a>

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

**Note**  
Mobile endpoints are not supported\.

#### Audit operation<a name="statement-operation-json-properties-audit"></a>

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

##### Required permissions when specifying log destinations<a name="required-permissions-log-operations"></a>

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

##### Audit destination log example<a name="data-protection-policy-audit-destination-log"></a>

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

##### Audit operation metrics<a name="data-protection-policy-audit-metrics"></a>

When an audit operation has specified the `FindingsDestination` or the `NoFindingsDestination` property, the topic owners also receive CloudWatch `MessagesWithFindings` and `MessagesWithNoFindings` metrics\.

![\[Example of an audit displaying data over a specified period of time.\]](http://docs.aws.amazon.com/sns/latest/dg/images/audit-operations-metrics.png)

## How do I determine the IAM principals for my data protection policy?<a name="data-protection-policy-iam-principal-determined"></a>

Message data protection uses two IAM principals that interact with Amazon SNS\.

1. **Publish API Principal** \(Inbound\) – The authenticated IAM principal calling the Amazon SNS `Publish` API\.

1. **Subscription Principal** \(Outbound\) – The authenticated IAM principal that called the `Subscribe` API during subscription creation\.

The `SubscriptionPrincipal` is a publicly available Amazon SNS subscription property that can be retrieved from the `GetSubscriptionAttributes` API\.

```
{
  "Attributes": {
    "SubscriptionPrincipal": "arn:aws:iam::123456789012:user/NoNameAccess",
    "Owner": "123412341234",
    "RawMessageDelivery": "true",
    "TopicArn": "arn:aws:sns:us-east-1:123412341234:PII-data-topic",
    "Endpoint": "arn:aws:sqs:us-east-1:123456789012:NoNameAccess",
    "Protocol": "sqs",
    "PendingConfirmation": "false",
    "ConfirmationWasAuthenticated": "true",
    "SubscriptionArn": "arn:aws:sns:us-east-1:123412341234:PII-data-topic:5d8634ef-67ef-49eb-a824-4042b28d6f55"
  }
}
```