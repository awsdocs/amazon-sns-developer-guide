# Protecting Amazon SNS Data Using Server\-Side Encryption \(SSE\) and AWS KMS<a name="sns-server-side-encryption"></a>

Server\-side encryption \(SSE\) lets you transmit sensitive data in encrypted topics\. SSE protects the contents of messages in Amazon SNS topics using keys managed in AWS Key Management Service \(AWS KMS\)\. 

For information about managing SSE using the AWS Management Console or the AWS SDK for Java \(by setting the `KmsMasterKeyId` attribute using the `[CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)` and `[SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html)` API actions\), see [Tutorial: Enabling Server\-Side Encryption \(SSE\) for an Amazon SNS Topic](sns-tutorial-enable-encryption-for-topic.md)\. For information about creating encrypted topics using AWS CloudFormation \(by setting the `KmsMasterKeyId` property using the `[AWS::SNS::Topic](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-sns-topic.html)` resource\), see the *AWS CloudFormation User Guide*\.

SSE encrypts messages as soon as Amazon SNS receives them\. The messages are stored in encrypted form and Amazon SNS decrypts messages only when they are sent\.

**Important**  
All requests to topics with SSE enabled must use HTTPS and [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.  
For information about compatibility of other services with encrypted topics, see your service documentation\.

AWS KMS combines secure, highly available hardware and software to provide a key management system scaled for the cloud\. When you use Amazon SNS with AWS KMS, the [data keys](#sse-key-terms) that encrypt your message data are also encrypted and stored with the data they protect\.

The following are benefits of using AWS KMS:
+ You can create and manage [customer master keys \(CMKs\)](#sse-key-terms) yourself\.
+ You can also use the AWS managed CMK for Amazon SNS, which is unique for each account and region\.
+ The AWS KMS security standards can help you meet encryption\-related compliance requirements\.

For more information, see [What is AWS Key Management Service?](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) in the *AWS Key Management Service Developer Guide* and the [AWS Key Management Service Cryptographic Details](https://d0.awsstatic.com/whitepapers/KMS-Cryptographic-Details.pdf) whitepaper\.

**Topics**
+ [Encryption Scope](#what-does-sse-encrypt)
+ [Key Terms](#sse-key-terms)
+ [Estimating AWS KMS Costs](#sse-estimate-kms-usage-costs)
+ [Configuring AWS KMS Permissions](#sns-what-permissions-for-sse)
+ [Errors](#sse-troubleshooting-errors)

## Encryption Scope<a name="what-does-sse-encrypt"></a>

SSE encrypts the body of a message in an Amazon SNS topic\.

SSE doesn't encrypt the following:
+ Topic metadata \(topic name and attributes\)
+ Message metadata \(subject, message ID, timestamp, and attributes\)
+ Per\-topic metrics

**Note**  
A message is encrypted only if it is sent after the encryption of a topic is enabled\. Amazon SNS doesn't encrypt backlogged messages\.
Any encrypted message remains encrypted even if the encryption of its topic is disabled\.

## Key Terms<a name="sse-key-terms"></a>

The following key terms can help you better understand the functionality of SSE\. For detailed descriptions, see the *[Amazon Simple Notification Service API Reference](https://docs.aws.amazon.com/sns/latest/api/)*\.

**Data Key**  
The data encryption key \(DEK\) responsible for encrypting the contents of Amazon SNS messages\.  
For more information, see [Data Keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#data-keys) in the *AWS Key Management Service Developer Guide* and [Envelope Encryption](https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/how-it-works.html#envelope-encryption) in the *AWS Encryption SDK Developer Guide*\.

**Customer Master Key ID**  
The alias, alias ARN, key ID, or key ARN of an AWS managed customer master key \(CMK\) or a custom CMK—in your account or in another account\. While the alias of the AWS managed CMK for Amazon SNS is always `alias/aws/sns`, the alias of a custom CMK can, for example, be `alias/MyAlias`\. You can use these CMKs to protect the messages in Amazon SNS topics\.   
Keep the following in mind:  
+ The first time you use the AWS Management Console to specify the AWS managed CMK for Amazon SNS for a topic, AWS KMS creates the AWS managed CMK for Amazon SNS\.
+ Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed CMK for Amazon SNS\.
You can create CMKs, define the policies that control how CMKs can be used, and audit CMK usage using the **Customer managed keys** section of the AWS KMS console or the `[CreateKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html)` AWS KMS action\. For more information, see [Customer Master Keys \(CMKs\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) and [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. For more examples of CMK identifiers, see [KeyId](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html#API_DescribeKey_RequestParameters) in the *AWS Key Management Service API Reference*\. For information about finding CMK identifiers, see [Find the Key ID and ARN](https://docs.aws.amazon.com/kms/latest/developerguide/viewing-keys.html#find-cmk-id-arn) in the *AWS Key Management Service Developer Guide*\.  
There are additional charges for using AWS KMS\. For more information, see [Estimating AWS KMS Costs](#sse-estimate-kms-usage-costs) and [AWS Key Management Service Pricing](https://aws.amazon.com/kms/pricing)\.

## Estimating AWS KMS Costs<a name="sse-estimate-kms-usage-costs"></a>

To predict costs and better understand your AWS bill, you might want to know how often Amazon SNS uses your customer master key \(CMK\)\.

**Note**  
Although the following formula can give you a very good idea of expected costs, actual costs might be higher because of the distributed nature of Amazon SNS\.

To calculate the number of API requests \(`R`\) *per topic*, use the following formula:

```
R = B / D * (2 * P)
```

`B` is the billing period \(in seconds\)\.

`D` is the data key reuse period \(in seconds—Amazon SNS reuses a data key for up to 5 minutes\)\.

`P` is the number of publishing [principals](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Principal) that send to the Amazon SNS topic\.

The following are example calculations\. For exact pricing information, see [AWS Key Management Service Pricing](https://aws.amazon.com/kms/pricing/)\.

### Example 1: Calculating the Number of AWS KMS API Calls for 1 Publisher and 1 Topic<a name="example-1-topic-1-publisher"></a>

This example assumes the following:
+ The billing period is January 1\-31 \(2,678,400 seconds\)\.
+ The data key reuse period is 5 minutes \(300 seconds\)\.
+ There is 1 topic\.
+ There is 1 publishing principal\.

```
2,678,400 / 300 * (2 * 1) = 17,856
```

### Example 2: Calculating the Number of AWS KMS API Calls for Multiple Publishers and 2 Topics<a name="example-2-topics-multiple-publishers"></a>

This example assumes the following:
+ The billing period is February 1\-28 \(2,419,200 seconds\)\.
+ The data key reuse period is 5 minutes \(300 seconds\)\.
+ There are 2 topics\.
+ The first topic has 3 publishing principals\.
+ The second topic has 5 publishing principals\.

```
(2,419,200 / 300 * (2 * 3)) + (2,419,200 / 300 * (2 * 5)) = 129,024
```

## Configuring AWS KMS Permissions<a name="sns-what-permissions-for-sse"></a>

Before you can use SSE, you must configure AWS KMS key policies to allow encryption of topics and encryption and decryption of messages\. For examples and more information about AWS KMS permissions, see [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.

**Note**  
You can also manage permissions for KMS keys using IAM policies\. For more information, see [Using IAM Policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html)\.  
While you can configure global permissions to send to and receive from Amazon SNS, AWS KMS requires explicitly naming the full ARN of CMKs in specific regions in the `Resource` section of an IAM policy\.

You must also ensure that the key policies of the customer master key \(CMK\) allow the necessary permissions\. To do this, name the principals that produce and consume encrypted messages in Amazon SNS as users in the CMK key policy\. 

Alternatively, you can specify the required AWS KMS actions and CMK ARN in an IAM policy assigned to the principals that publish and subscribe to receive encrypted messages in Amazon SNS\. For more information, see [Managing Access to AWS KMS CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the *AWS Key Management Service Developer Guide*\.

### Allow a User to Send Messages to a Topic with SSE<a name="send-to-encrypted-topic"></a>

The publisher must have the `kms:GenerateDataKey` and `kms:Decrypt` permissions for the customer master key \(CMK\)\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "kms:GenerateDataKey",
      "kms:Decrypt"
    ],
    "Resource": "arn:aws:kms:us-east-2:123456789012:key/1234abcd-12ab-34cd-56ef-1234567890ab"
  }, {
    "Effect": "Allow",
    "Action": [
      "sns:Publish"
    ],
    "Resource": "arn:aws:sns:*:123456789012:MyTopic"
  }]
}
```

### Enable Compatibility between Event Sources from AWS Services and Encrypted Topics<a name="compatibility-with-aws-services"></a>

Several AWS services publish events to Amazon SNS topics\. To allow these event sources to work with encrypted topics, you must perform the following steps:

1. [Create a customer master key \(CMK\)\.](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-keys-console)

1. To allow the AWS service feature to have the `kms:GenerateDataKey*` and `kms:Decrypt` permissions, add the following statement to the policy of the CMK using the correct service principal\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [{
       "Effect": "Allow",
       "Principal": {
         "Service": "service.amazonaws.com"
       },
       "Action": [
         "kms:GenerateDataKey*",
         "kms:Decrypt"
       ],
       "Resource": "*"
     }]
   }
   ```    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-server-side-encryption.html)
**Note**  
Some Amazon SNS event sources require you to provide an IAM role \(rather than the service principal\) in the AWS KMS key policy:  
[Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ASGettingNotifications.html)
[Amazon Elastic Transcoder](https://docs.aws.amazon.com/elastictranscoder/latest/developerguide/notifications.html)
[AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals.html#approvals-configuration-options)
[AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/notifications-for-AWS-Config.html)
[AWS Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.sns.html)
[AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sns-rule.html)

1. [Enable SSE for your topic](sns-tutorial-enable-encryption-for-topic.md) using your CMK\.

1. Provide the ARN of the encrypted topic to the event source\.

## Errors<a name="sse-troubleshooting-errors"></a>

When you work with Amazon SNS and AWS KMS, you might encounter errors\. The following list describes the errors and possible troubleshooting solutions\.

**KMSAccessDeniedException**  
The ciphertext references a key that doesn't exist or that you don't have access to\.  
HTTP Status Code: 400

**KMSDisabledException**  
The request was rejected because the specified CMK isn't enabled\.  
HTTP Status Code: 400

**KMSInvalidStateException**  
The request was rejected because the state of the specified resource isn't valid for this request\. For more information, see [How Key State Affects Use of a Customer Master Key](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html) in the *AWS Key Management Service Developer Guide*\.  
HTTP Status Code: 400

**KMSNotFoundException**  
The request was rejected because the specified entity or resource can't be found\.  
HTTP Status Code: 400

**KMSOptInRequired**  
The AWS access key ID needs a subscription for the service\.  
HTTP Status Code: 403

**KMSThrottlingException**  
The request was denied due to request throttling\. For more information about throttling, see [Limits](https://docs.aws.amazon.com/kms/latest/developerguide/limits.html#requests-per-second) in the *AWS Key Management Service Developer Guide*\.  
HTTP Status Code: 400