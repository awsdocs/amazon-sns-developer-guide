# Creating data protection policies to secure message data \(API\)<a name="sns-message-data-protection-configure-api"></a>

The number and size of Amazon SNS resources in an AWS account are limited\. For more information, see [Amazon Simple Notification Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html)\.

## Creating data protection policies \(AWS API\)<a name="create-policies-api"></a>

You can create an Amazon SNS data protection policy using the AWS API\.

**To create a data protection policy together with an Amazon SNS topic \(AWS API\)**  
Use the `DataProtectionPolicy` property of a standard Amazon SNS topic:
+ [https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)

**To retrieve or create a data protection policy for an existing Amazon SNS topic \(AWS API\)**  
Call one of the following operations:
+ [GetDataProtectionPolicy](https://docs.aws.amazon.com/sns/latest/api/API_GetDataProtectionPolicy.html)
+ [PutDataProtectionPolicy](https://docs.aws.amazon.com/sns/latest/api/API_PutDataProtectionPolicy.html)