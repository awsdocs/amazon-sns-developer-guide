# Message data protection<a name="message-data-protection"></a>

**Topics**
+ [What is message data protection?](#what-is-message-data-protection)
+ [Why should I use message data protection?](#why-use-message-data-protection)
+ [Understanding data protection policies](sns-message-data-protection-policies.md)
+ [Using managed data identifiers in Amazon SNS](sns-message-data-protection-data-identifiers.md)

## What is message data protection?<a name="what-is-message-data-protection"></a>

**Message data protection** safeguards the data that's published to your Amazon SNS topics by using [data protection policies](sns-message-data-protection-policies.md) to audit, mask, redact or block the sensitive information that moves between applications or AWS services\.

Message data protection scans data in motion for personally identifiable information \(PII\) and protected health information \(PHI\) using predefined *data identifiers* \(for example, names, addresses, credit card numbers, and prescription drug codes\)\. Using the scanned information, message data protection provides detailed audit logs, and allows you to take action to protect that data\.

Message data protection supports the following actions to help protect sensitive customer information:
+ [**Audit**](sns-message-data-protection-operations.md#statement-operation-json-properties-audit) – Audit up to 99% of the data that's published to an Amazon SNS topic\. You can then choose to send the findings to [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html), [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html), or [Amazon Kinesis Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)\.
+ [**De\-identify**](sns-message-data-protection-operations.md#statement-operation-json-properties-deidentify) – Mask or redact sensitive data without interrupting message publishing\.
+ [**Deny**](sns-message-data-protection-operations.md#statement-operation-json-properties-deny) – Block the transmission of data between applications and AWS resources if sensitive data is present within the payload\.

## Why should I use message data protection?<a name="why-use-message-data-protection"></a>

By introducing message data protection into your governance, risk management, and compliance programs, you can implement data protection policies that help you to identify and prevent data leakage\. This provides your teams with tools that can help to reduce financial, legal, and regulatory risks by complying with privacy regulations such as HIPAA, GDPR, PCI, and FedRAMP\. It also frees your developers from the operational overhead that's associated with building and managing your own tools to protect sensitive data\.

For example, you can use message data protection to create an *audit policy* to determine whether any of your systems are inadvertently sending or receiving sensitive data\. If your audit results show that systems are sending credit card information to systems that don’t require it, you can use a *block policy* to prevent the delivery of the data\. 

**Note**  
Amazon SNS supports message data protection for Amazon SNS standard topics only\.  
Message data protection does not support the Amazon SNS `PublishBatch` API for inbound messages during preview\. Message delivery for `PublishBatch` API is still protected\.