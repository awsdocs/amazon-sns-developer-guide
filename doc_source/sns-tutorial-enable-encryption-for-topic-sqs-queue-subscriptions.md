# Tutorial: Enabling server\-side encryption \(SSE\) for an Amazon SNS topic with an encrypted Amazon SQS queue subscribed<a name="sns-tutorial-enable-encryption-for-topic-sqs-queue-subscriptions"></a>

You can enable server\-side encryption \(SSE\) for a topic to protect its data\. To allow Amazon SNS to send messages to encrypted Amazon SQS queues, the customer master key \(CMK\) associated with the Amazon SQS queue must have a policy statement that grants Amazon SNS service\-principal access to the AWS KMS API actions `GenerateDataKey` and `Decrypt`\. Because AWS managed CMKs don't support policy modifications, you must use a custom CMK\. For more information about using SSE, see [Encryption at rest](sns-server-side-encryption.md)\.

The following tutorial shows how you can enable SSE for an Amazon SNS topic to which an encrypted Amazon SQS queue is subscribed, using the AWS Management Console\.

## Step 1: To create a custom CMK<a name="create-custom-cmk"></a>

1. Sign in to the [AWS KMS console](https://console.aws.amazon.com/kms/) with a user that has at least the `AWSKeyManagementServicePowerUser` policy\.

1. Choose **Create a key**\.

1. On the **Add alias and description** page, enter an **Alias** for your key \(for example, `MyCustomCMK`\) and then choose **Next**\.

1. On the **Add tags** page, choose **Next**\.

1. On the **Define key administrative permissions** page, in the **Key administrators** section, choose an IAM role or an IAM user and then choose **Next**\.

1. On the **Define key usage permissions** page, in the **This account** section, choose an IAM role or an IAM user and then choose **Next**\.

1. On the **Review and edit key policy** page, add the following statement to the key policy, and then choose **Finish**\.

   ```
   {
       "Sid": "Allow Amazon SNS to use this key",
       "Effect": "Allow",
       "Principal": {
           "Service": "sns.amazonaws.com"
       },
       "Action": [
           "kms:Decrypt",
           "kms:GenerateDataKey*"
       ],
       "Resource": "*"
   }
   ```

Your new custom CMK appears in the list of keys\.

## Step 2: To create an encrypted Amazon SNS topic<a name="create-encrypted-topic"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. Choose **Create topic**\.

1. On the **Create new topic** page, for **Name**, enter a topic name \(for example, `MyEncryptedTopic`\) and then choose **Create topic**\.

1. Expand the **Encryption** section and do the following: 

   1. Choose **Enable server\-side encryption**\.

   1. Specify the customer master key \(CMK\)\. For more information, see [Key terms](sns-server-side-encryption.md#sse-key-terms)\.

      For each CMK type, the **Description**, **Account**, and **CMK ARN** are displayed\.
**Important**  
If you aren't the owner of the CMK, or if you log in with an account that doesn't have the `kms:ListAliases` and `kms:DescribeKey` permissions, you won't be able to view information about the CMK on the Amazon SNS console\.  
Ask the owner of the CMK to grant you these permissions\. For more information, see the [AWS KMS API Permissions: Actions and Resources Reference](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html) in the *AWS Key Management Service Developer Guide*\.

   1. For **Customer master key \(CMK\)**, choose **MyCustomCMK** [which you created earlier](#create-custom-cmk) and then choose **Enable server\-side encryption**\.

1. Choose **Save changes**\.

   SSE is enabled for your topic and the **MyTopic** page is displayed\.

   The topic's **Encryption** status, AWS **Account**, **Customer master key \(CMK\)**, **CMK ARN**, and **Description** are displayed on the **Encryption** tab\.

Your new encrypted topic appears in the list of topics\.

## Step 3: To create and subscribe encrypted Amazon SQS queues<a name="create-encrypted-queue"></a>

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. Choose **Create New Queue**\.

1. On the **Create New Queue** page, do the following:

   1. Enter a **Queue Name** \(for example, `MyEncryptedQueue1`\)\.

   1. Choose **Standard Queue**, and then choose **Configure Queue**\.

   1. Choose **Use SSE**\.

   1. For **AWS AWS KMS Customer Master Key \(CMK\)**, choose **MyCustomCMK** [which you created earlier](#create-custom-cmk), and then choose **Create Queue**\.

1. Repeat the process to create a second queue \(for example, named `MyEncryptedQueue2`\)\.

   Your new encrypted queues appear in the list of queues\.

1. On the Amazon SQS console, choose `MyEncryptedQueue1` and `MyEncryptedQueue2` and then choose **Queue Actions**, **Subscribe Queues to SNS Topic**\.

1. In the **Subscribe to a Topic** dialog box, for **Choose a Topic** select **MyEncryptedTopic**, and then choose **Subscribe**\.

   Your encrypted queues' subscriptions to your encrypted topic are displayed in the **Topic Subscription Result** dialog box\.

1. Choose **OK**\.

## Step 4: To publish a message to your encrypted topic<a name="publish-to-encrypted-topic"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. From the list of topics, choose **MyEncryptedTopic** and then choose **Publish message**\.

1. On the **Publish a message** page, do the following:

   1. \(Optional\) In the **Message details** section, enter the **Subject** \(for example, `Testing message publishing`\)\.

   1. In the **Message body** section, enter the message body \(for example, `My message body is encrypted at rest.`\)\.

   1. Choose **Publish message**\.

Your message is published to your subscribed encrypted queues\.

## Step 5: To verify message delivery<a name="verify-message-delivery"></a>

1. Sign in to the [Amazon SQS console](https://console.aws.amazon.com/sqs/)\.

1. From the list of queues, choose **MyEncryptedQueue1** and then choose **Queue Actions**, **View/Delete Messages**\.

1. On the **View/Delete Messages in MyEncryptedQueue1** page, choose **Start polling for messages**\.

   The message [that you sent earlier](#publish-to-encrypted-topic) is displayed\.

1. Choose **More Details** to view your message\.

1. When you're finished, choose **Close**\.

1. Repeat the process for **MyEncryptedQueue2**\.