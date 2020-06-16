# Encryption at rest<a name="sns-server-side-encryption"></a>

Server\-side encryption \(SSE\) lets you store sensitive data in encrypted topics\. SSE protects the contents of messages in Amazon SNS topics using keys managed in AWS Key Management Service \(AWS KMS\)\. 

For information about managing SSE using the AWS Management Console or the AWS SDK for Java \(by setting the `KmsMasterKeyId` attribute using the `[CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)` and `[SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html)` API actions\), see [Tutorial: Enabling server\-side encryption \(SSE\) for an Amazon SNS topic](sns-tutorial-enable-encryption-for-topic.md)\. For information about creating encrypted topics using AWS CloudFormation \(by setting the `KmsMasterKeyId` property using the `[AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html)` resource\), see the *AWS CloudFormation User Guide*\.

SSE encrypts messages as soon as Amazon SNS receives them\. The messages are stored in encrypted form and Amazon SNS decrypts messages only when they are sent\.

**Important**  
All requests to topics with SSE enabled must use HTTPS and [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.  
For information about compatibility of other services with encrypted topics, see your service documentation\.

AWS KMS combines secure, highly available hardware and software to provide a key management system scaled for the cloud\. When you use Amazon SNS with AWS KMS, the [data keys](#sse-key-terms) that encrypt your message data are also encrypted and stored with the data they protect\.

The following are benefits of using AWS KMS:
+ You can create and manage [customer master keys \(CMKs\)](#sse-key-terms) yourself\.
+ You can also use AWS\-managed KMS keys for Amazon SNS, which are unique for each account and region\.
+ The AWS KMS security standards can help you meet encryption\-related compliance requirements\.

For more information, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide* and the [AWS Key Management Service Cryptographic Details](https://d0.awsstatic.com/whitepapers/KMS-Cryptographic-Details.pdf) whitepaper\.

**Topics**
+ [Encryption scope](#what-does-sse-encrypt)
+ [Key terms](#sse-key-terms)

## Encryption scope<a name="what-does-sse-encrypt"></a>

SSE encrypts the body of a message in an Amazon SNS topic\.

SSE doesn't encrypt the following:
+ Topic metadata \(topic name and attributes\)
+ Message metadata \(subject, message ID, timestamp, and attributes\)
+ Per\-topic metrics

**Note**  
A message is encrypted only if it is sent after the encryption of a topic is enabled\. Amazon SNS doesn't encrypt backlogged messages\.
Any encrypted message remains encrypted even if the encryption of its topic is disabled\.

## Key terms<a name="sse-key-terms"></a>

The following key terms can help you better understand the functionality of SSE\. For detailed descriptions, see the *[Amazon Simple Notification Service API Reference](https://docs.aws.amazon.com/sns/latest/api/)*\.

**Data key**  
The data encryption key \(DEK\) responsible for encrypting the contents of Amazon SNS messages\.  
For more information, see [Data Keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#data-keys) in the *AWS Key Management Service Developer Guide* and [Envelope Encryption](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/how-it-works.html#envelope-encryption) in the *AWS Encryption SDK Developer Guide*\.

**Customer master key ID**  
The alias, alias ARN, key ID, or key ARN of an AWS managed customer master key \(CMK\) or a custom CMK—in your account or in another account\. While the alias of the AWS managed CMK for Amazon SNS is always `alias/aws/sns`, the alias of a custom CMK can, for example, be `alias/MyAlias`\. You can use these CMKs to protect the messages in Amazon SNS topics\.   
Keep the following in mind:  
+ The first time you use the AWS Management Console to specify the AWS managed CMK for Amazon SNS for a topic, AWS KMS creates the AWS managed CMK for Amazon SNS\.
+ Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed CMK for Amazon SNS\.
You can create CMKs, define the policies that control how CMKs can be used, and audit CMK usage using the **Customer managed keys** section of the AWS KMS console or the `[CreateKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html)` AWS KMS action\. For more information, see [Customer Master Keys \(CMKs\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) and [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. For more examples of CMK identifiers, see [KeyId](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html#API_DescribeKey_RequestParameters) in the *AWS Key Management Service API Reference*\. For information about finding CMK identifiers, see [Find the Key ID and ARN](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys.html#find-cmk-id-arn) in the *AWS Key Management Service Developer Guide*\.  
There are additional charges for using AWS KMS\. For more information, see [Estimating AWS KMS costs](sns-key-management.md#sse-estimate-kms-usage-costs) and [AWS Key Management Service Pricing](https://aws.amazon.com/kms/pricing)\.