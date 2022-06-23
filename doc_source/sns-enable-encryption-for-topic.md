# Enabling server\-side encryption \(SSE\) for an Amazon SNS topic<a name="sns-enable-encryption-for-topic"></a>

You can enable server\-side encryption \(SSE\) for a topic to protect its data\. For more information about using SSE, see [Encryption at rest](sns-server-side-encryption.md)\.

**Important**  
All requests to topics with SSE enabled must use HTTPS and [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.

This page shows how to enable, disable, and configure SSE for an existing Amazon SNS topic using the AWS Management Console\.

## To enable server\-side encryption \(SSE\) for an Amazon SNS topic using the AWS Management Console<a name="enable-encryption-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and choose **Actions**, **Edit**\.

1. Expand the **Encryption** section and do the following: 

   1. Choose **Enable encryption**\.

   1. Specify the AWS KMS key\. For more information, see [Key terms](sns-server-side-encryption.md#sse-key-terms)\.

      For each KMS type, the **Description**, **Account**, and **KMS ARN** are displayed\.
**Important**  
If you aren't the owner of the KMS, or if you log in with an account that doesn't have the `kms:ListAliases` and `kms:DescribeKey` permissions, you won't be able to view information about the KMS on the Amazon SNS console\.  
Ask the owner of the KMS to grant you these permissions\. For more information, see the [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.
      + The AWS managed KMS for Amazon SNS **\(Default\) alias/aws/sns** is selected by default\.
**Note**  
Keep the following in mind:  
The first time you use the AWS Management Console to specify the AWS managed KMS for Amazon SNS for a topic, AWS KMS creates the AWS managed KMS for Amazon SNS\.
Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed KMS for Amazon SNS\.
      + To use a custom KMS from your AWS account, choose the **AWS KMS key** field and then choose the custom KMS from the list\.
**Note**  
For instructions on creating custom KMSs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*
      + To use a custom KMS ARN from your AWS account or from another AWS account, enter it into the **AWS KMS key** field\.

1. Choose **Save changes**\.

   SSE is enabled for your topic and the ***MyTopic*** page is displayed\.

   The topic's **Encryption** status, AWS **Account**, **Customer master key \(CMK\)**, **CMK ARN**, and **Description** are displayed on the **Encryption** tab\.