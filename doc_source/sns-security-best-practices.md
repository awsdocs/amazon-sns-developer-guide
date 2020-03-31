# Amazon SNS security best practices<a name="sns-security-best-practices"></a>

AWS provides many security features for Amazon SNS\. Review these security features in the context of your own security policy\.

**Note**  
The guidance for these security features applies to common use cases and implementations\. We recommend that you review these best practices in the context of your specific use case, architecture, and threat model\.

## Preventative best practices<a name="preventative-best-practices"></a>

The following are preventative security best practices for Amazon SNS\.

**Topics**
+ [Ensure topics aren't publicly accessible](#ensure-topics-not-publicly-accessible)
+ [Implement least\-privilege access](#implement-least-privilege-access)
+ [Use IAM roles for applications and AWS services which require Amazon SNS access](#use-iam-roles-for-applications-aws-services-which-require-access)
+ [Implement server\-side encryption](#implement-server-side-encryption)
+ [Enforce encryption of data in transit](#enforce-encryption-data-in-transit)
+ [Consider using VPC endpoints to access Amazon SNS](#consider-using-vpc-endpoints-access-sns)

### Ensure topics aren't publicly accessible<a name="ensure-topics-not-publicly-accessible"></a>

Unless you explicitly require anyone on the internet to be able to read or write to your Amazon SNS topic, you should ensure that your topic isn't publicly accessible \(accessible by everyone in the world or by any authenticated AWS user\)\.
+ Avoid creating policies with `Principal` set to `""`\.
+ Avoid using a wildcard \(`*`\)\. Instead, name a specific user or users\.

### Implement least\-privilege access<a name="implement-least-privilege-access"></a>

When you grant permissions, you decide who receives them, which topics the permissions are for, and specific API actions that you want to allow for these topics\. Implementing the principle of least privilege is important to reducing security risks\. It also helps to reduce the negative effect of errors or malicious intent\.

Follow the standard security advice of granting least privilege\. That is, grant only the permissions required to perform a specific task\. You can implement least privilege by using a combination of security policies pertaining to user access\.

Amazon SNS uses the publisher\-subscriber model, requiring three types of user account access:
+ **Administrators** – Access to creating, modifying, and deleting topics\. Administrators also control topic policies\.
+ **Publishers** – Access to sending messages to topics\.
+ **Subscribers** – Access to subscribing to topics\.

For more information, see the following sections:
+ [Identity and access management in Amazon SNS](sns-authentication-and-access-control.md)
+ [Amazon SNS API permissions: Actions and resources reference](sns-access-policy-language-api-permissions-reference.md)

### Use IAM roles for applications and AWS services which require Amazon SNS access<a name="use-iam-roles-for-applications-aws-services-which-require-access"></a>

For applications or AWS services, such as Amazon EC2, to access Amazon SNS topics, they must use valid AWS credentials in their AWS API requests\. Because these credentials aren't rotated automatically, you shouldn't store AWS credentials directly in the application or EC2 instance\.

You should use an IAM role to manage temporary credentials for applications or services that need to access Amazon SNS\. When you use a role, you don't need to distribute long\-term credentials \(such as a user name, password, and access keys\) to an EC2 instance or AWS service, such as AWS Lambda\. Instead, the role supplies temporary permissions that applications can use when they make calls to other AWS resources\.

For more information, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) and [Common Scenarios for Roles: Users, Applications, and Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios.html) in the *IAM User Guide*\.

### Implement server\-side encryption<a name="implement-server-side-encryption"></a>

To mitigate data leakage issues, use encryption at rest to encrypt your messages using a key stored in a different location from the location that stores your messages\. Server\-side encryption \(SSE\) provides data encryption at rest\. Amazon SNS encrypts your data at the message level when it stores it, and decrypts the messages for you when you access them\. SSE uses keys managed in AWS Key Management Service\. When you authenticate your request and have access permissions, there is no difference between accessing encrypted and unencrypted topics\.

For more information, see [Encryption at rest](sns-server-side-encryption.md) and [Key management](sns-key-management.md)\.

### Enforce encryption of data in transit<a name="enforce-encryption-data-in-transit"></a>

Without HTTPS \(TLS\), a network\-based attacker can eavesdrop on network traffic or manipulate it using an attack such as man\-in\-the\-middle\. Allow only encrypted connections over HTTPS \(TLS\) using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_Boolean](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html#Conditions_Boolean) condition in the topic policy to force requests to use SSL\. 

### Consider using VPC endpoints to access Amazon SNS<a name="consider-using-vpc-endpoints-access-sns"></a>

If you have topics that you must be able to interact with, but these topics must absolutely not be exposed to the internet, use VPC endpoints to limit topic access to only the hosts within a particular VPC\. You can use topic policies to control access to topics from specific Amazon VPC endpoints or from specific VPCs\.

Amazon SNS VPC endpoints provide two ways to control access to your messages:
+ You can control the requests, users, or groups that are allowed through a specific VPC endpoint\.
+ You can control which VPCs or VPC endpoints have access to your topic using a topic policy\.

For more information, see [Creating an Amazon VPC endpoint for Amazon SNS](sns-internetwork-traffic-privacy.md#sns-vpc-endpoint) and [Creating an Amazon VPC endpoint policy for Amazon SNS](sns-internetwork-traffic-privacy.md#sns-vpc-endpoint-policy)\.