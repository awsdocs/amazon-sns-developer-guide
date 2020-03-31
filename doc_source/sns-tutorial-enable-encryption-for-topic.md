# Tutorial: Enabling server\-side encryption \(SSE\) for an Amazon SNS topic<a name="sns-tutorial-enable-encryption-for-topic"></a>

You can enable server\-side encryption \(SSE\) for a topic to protect its data\. For more information about using SSE, see [Encryption at rest](sns-server-side-encryption.md)\.

**Important**  
All requests to topics with SSE enabled must use HTTPS and [Signature Version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\.

The following tutorial shows how to enable, disable, and configure SSE for an existing Amazon SNS topic using the AWS Management Console and the AWS SDK for Java \(by setting the `KmsMasterKeyId` attribute using the `[CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)` and `[SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html)` API actions\)\.

**Topics**
+ [AWS Management Console](#enable-encryption-console)
+ [AWS SDK for Java](#enable-encryption-aws-java)

## To enable server\-side encryption \(SSE\) for an Amazon SNS topic using the AWS Management Console<a name="enable-encryption-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and choose **Actions**, **Edit**\.

1. Expand the **Encryption** section and do the following: 

   1. Choose **Enable encryption**\.

   1. Specify the customer master key \(CMK\)\. For more information, see [Key terms](sns-server-side-encryption.md#sse-key-terms)\.

      For each CMK type, the **Description**, **Account**, and **CMK ARN** are displayed\.
**Important**  
If you aren't the owner of the CMK, or if you log in with an account that doesn't have the `kms:ListAliases` and `kms:DescribeKey` permissions, you won't be able to view information about the CMK on the Amazon SNS console\.  
Ask the owner of the CMK to grant you these permissions\. For more information, see the [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.
      + The AWS managed CMK for Amazon SNS **\(Default\) alias/aws/sns** is selected by default\.
**Note**  
Keep the following in mind:  
The first time you use the AWS Management Console to specify the AWS managed CMK for Amazon SNS for a topic, AWS KMS creates the AWS managed CMK for Amazon SNS\.
Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed CMK for Amazon SNS\.
      + To use a custom CMK from your AWS account, choose the **Customer master key \(CMK\)** field and then choose the custom CMK from the list\.
**Note**  
For instructions on creating custom CMKs, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*
      + To use a custom CMK ARN from your AWS account or from another AWS account, enter it into the **Customer master key \(CMK\)** field\.

1. Choose **Save changes**\.

   SSE is enabled for your topic and the ***MyTopic*** page is displayed\.

   The topic's **Encryption** status, AWS **Account**, **Customer master key \(CMK\)**, **CMK ARN**, and **Description** are displayed on the **Encryption** tab\.

## To enable server\-side encryption \(SSE\) for an Amazon SNS topic using the AWS SDK for Java<a name="enable-encryption-aws-java"></a>

1. Configure AWS KMS key policies to allow encryption of topics and encryption and decryption of messages\. For more information, see [Configuring AWS KMS permissions](sns-key-management.md#sns-what-permissions-for-sse)

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Obtain the customer master key \(CMK\) ID\. For more information, see [Key terms](sns-server-side-encryption.md#sse-key-terms)\.
**Note**  
Keep the following in mind:  
The first time you use the AWS Management Console to specify the AWS managed CMK for Amazon SNS for a topic, AWS KMS creates the AWS managed CMK for Amazon SNS\.
Alternatively, the first time you use the `Publish` action on a topic with SSE enabled, AWS KMS creates the AWS managed CMK for Amazon SNS\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   To enable server\-side encryption, specify the CMK ID by setting the `KmsMasterKeyId` attribute using the `[CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)` or `[SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html)` action\.

   The following code excerpt enables SSE for an existing topic using the AWS managed CMK for Amazon SNS:

   ```
   // Enable server-side encryption by specifying the alias ARN of the AWS managed CMK for Amazon SNS.
   final String kmsMasterKeyAlias = "arn:aws:kms:us-east-2:123456789012:alias/aws/sns";
   
   final SetTopicAttributesRequest setAttributesRequest = new SetTopicAttributesRequest()
       .withTopicArn(topicArn)
       .withAttributeName("KmsMasterKeyId")
       .withAttributeValue(kmsMasterKeyAlias);           
   
   final SetTopicAttributesResponse setAttributesResponse = snsClient.setTopicAttributes(setAttributesRequest)
   ```

   To disable server\-side encryption for an existing topic, set the `KmsMasterKeyId` attribute to an empty string using the `SetTopicAttributes` action\.
**Important**  
`null` isn't a valid value for `KmsMasterKeyId`\.

   The following code excerpt creates a new topic with SSE using a custom CMK: 

   ```
   final Map<String, String> attributes = new HashMap<String, String>();
   
   // Enable server-side encryption by specifying the alias ARN of the custom CMK.
   final String kmsMasterKeyAlias = "arn:aws:kms:us-east-2:123456789012:alias/MyAlias";
   attributes.put("KmsMasterKeyId", kmsMasterKeyAlias);
    
   final CreateTopicRequest createRequest = new CreateTopicRequest("MyTopic")
       .withAttributes(attributes);
    
   final CreateTopicRespone createResponse = snsClient.createTopic(createRequest);
   ```