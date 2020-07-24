# Key management<a name="sns-key-management"></a>

The following sections provide information about working with keys managed in AWS Key Management Service \(AWS KMS\)\.

**Topics**
+ [Estimating AWS KMS costs](#sse-estimate-kms-usage-costs)
+ [Configuring AWS KMS permissions](#sns-what-permissions-for-sse)
+ [AWS KMS errors](#sse-troubleshooting-errors)

## Estimating AWS KMS costs<a name="sse-estimate-kms-usage-costs"></a>

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

### Example 1: Calculating the number of AWS KMS API calls for 1 publisher and 1 topic<a name="example-1-topic-1-publisher"></a>

This example assumes the following:
+ The billing period is January 1\-31 \(2,678,400 seconds\)\.
+ The data key reuse period is 5 minutes \(300 seconds\)\.
+ There is 1 topic\.
+ There is 1 publishing principal\.

```
2,678,400 / 300 * (2 * 1) = 17,856
```

### Example 2: Calculating the number of AWS KMS API calls for multiple publishers and 2 topics<a name="example-2-topics-multiple-publishers"></a>

This example assumes the following:
+ The billing period is February 1\-28 \(2,419,200 seconds\)\.
+ The data key reuse period is 5 minutes \(300 seconds\)\.
+ There are 2 topics\.
+ The first topic has 3 publishing principals\.
+ The second topic has 5 publishing principals\.

```
(2,419,200 / 300 * (2 * 3)) + (2,419,200 / 300 * (2 * 5)) = 129,024
```

## Configuring AWS KMS permissions<a name="sns-what-permissions-for-sse"></a>

Before you can use SSE, you must configure AWS KMS key policies to allow encryption of topics and encryption and decryption of messages\. For examples and more information about AWS KMS permissions, see [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.

**Note**  
You can also manage permissions for KMS keys using IAM policies\. For more information, see [Using IAM Policies with AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html)\.  
While you can configure global permissions to send to and receive from Amazon SNS, AWS KMS requires explicitly naming the full ARN of CMKs in specific regions in the `Resource` section of an IAM policy\.

You must also ensure that the key policies of the customer master key \(CMK\) allow the necessary permissions\. To do this, name the principals that produce and consume encrypted messages in Amazon SNS as users in the CMK key policy\. 

Alternatively, you can specify the required AWS KMS actions and CMK ARN in an IAM policy assigned to the principals that publish and subscribe to receive encrypted messages in Amazon SNS\. For more information, see [Managing Access to AWS KMS CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the *AWS Key Management Service Developer Guide*\.

### Allow a user to send messages to a topic with SSE<a name="send-to-encrypted-topic"></a>

The publisher must have the `kms:GenerateDataKey` and `kms:Decrypt` permissions for the customer master key \(CMK\)\.

```
{
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

### Enable compatibility between event sources from AWS services and encrypted topics<a name="compatibility-with-aws-services"></a>

Several AWS services publish events to Amazon SNS topics\. To allow these event sources to work with encrypted topics, you must perform the following steps\.

1. Use a customer managed CMK\. For more information, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.

1. To allow the AWS service to have the `kms:GenerateDataKey*` and `kms:Decrypt` permissions, add the following statement to the CMK policy\.

   ```
   {
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
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-key-management.html)
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

## AWS KMS errors<a name="sse-troubleshooting-errors"></a>

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