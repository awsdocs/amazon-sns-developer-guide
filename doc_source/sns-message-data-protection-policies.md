# Understanding data protection policies<a name="sns-message-data-protection-policies"></a>

**Topics**
+ [What are data protection policies?](#what-are-data-protection-policies)
+ [How is the data protection policy structured?](#overview-of-data-protection-policies)
+ [How do I determine the IAM principals for my data protection policy?](#data-protection-policy-iam-principal-determined)
+ [Data protection policy operations](sns-message-data-protection-operations.md)
+ [Data protection policy examples](sns-message-data-protection-examples.md)
+ [Creating data protection policies](sns-message-data-protection-configure.md)
+ [Deleting data protection policies in Amazon SNS](sns-message-data-protection-delete.md)

## What are data protection policies?<a name="what-are-data-protection-policies"></a>

Amazon SNS uses **data protection policies** to select the sensitive data for which you want to scan, and the actions that you want to take to protect that data from being exchanged by your Amazon SNS topics\. To select the sensitive data of interest, you use [data identifiers](sns-message-data-protection-data-identifiers.md)\. Amazon SNS message data protection then detects the sensitive data by using machine learning and pattern matching\. To act upon data identifiers that are found, you can define an **audit**, **de\-identify**, or **deny** operation\. These operations let you log the sensitive data that is found \(or not found\), mask or redact sensitive data, or deny message delivery\.

![\[Message data protection uses data protection policies to monitor sensitive data going into and out of your Amazon SNS topic.\]](http://docs.aws.amazon.com/sns/latest/dg/images/message-data-protection-policies-overview.png)

## How is the data protection policy structured?<a name="overview-of-data-protection-policies"></a>

As illustrated in the following figure, a data protection policy document includes the following elements:
+ Optional policy\-wide information at the top of the document
+ One or more individual statements

Each statement includes information about a single permission\.

![\[Statement to policy statement example\]](http://docs.aws.amazon.com/sns/latest/dg/images/payload-policy-process.png)

Only one data protection policy can be defined per Amazon SNS topic\. The data protection policy can have one or more deny or de\-identify statements, but only one audit statement\.

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
+ **Operation** – The follow\-on action, either **Audit**, **De\-identify** \(mask or redact\), or **Deny** \(block\), which the Amazon SNS topic executes once it finds sensitive data\.

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
+ [**Audit**](sns-message-data-protection-operations.md#statement-operation-json-properties-audit) – Emits metrics and finding logs without interrupting message publishing or delivery\.
+ [**De\-identify**](sns-message-data-protection-operations.md#statement-operation-json-properties-deidentify) – Mask or redact sensitive data without interrupting message publishing\.
+ [**Deny**](sns-message-data-protection-operations.md#statement-operation-json-properties-deny) – Blocks the Amazon SNS publish request or fails the message delivery\.

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