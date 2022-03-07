# 10DLC cross\-account access<a name="sns-settings-sms-crossaccount-10dlc"></a>

Each 10DLC phone number is associated with a single account in a single AWS Region\. If you want to use the same 10DLC phone number to send messages in more than one account or Region, you have two options:

1. You can register the same company and campaign in each of your AWS accounts\. These registrations are managed and charged separately\. If you register the same company in multiple AWS accounts, the number of messages that you can send to T\-Mobile customers per day is shared across each of those accounts\.

1. You can complete the 10DLC registration process in one AWS account, and use AWS Identity and Access Management \(IAM\) to grant other accounts permission to send through your 10DLC number\.
**Note**  
This option allows for true cross\-account access to your 10DLC phone numbers\. However, note that messages sent from your secondary accounts are treated as if they were sent from your primary account\. Quotas and billing are counted against the primary account and not against any secondary accounts\.

## Setting up cross\-account access using IAM policies<a name="sns-settings-sms-crossaccount-iam"></a>

You can use IAM roles to associate other accounts with your main account\. Then, you can delegate access permissions from your primary account to your secondary accounts by granting them access to the 10DLC numbers in the primary account\.

**To grant access to a 10DLC number in your primary account**

1. If you haven't already done so, complete the 10DLC registration process in the primary account\. This process involves three steps:
   + Register your company\. For more information, see [Registering your company or brand](sns-settings-register-company.md#sns-register-company-steps-10dlc) for use with 10DLC\.
   + Register your 10DLC campaign \(use case\)\. For more information, see [Registering a 10DLC campaign](sns-settings-register-campaign-10dlc.md)\.
   + Associate a phone number with your 10DLC campaign\. For more information, see [Associating a long code with a 10DLC campaign](sns-settings-associate-long-code-10dlc.md)\.

1. Create an IAM role in your primary account that allows another account to call the `Publish` API operation for your 10DLC phone number\. For more information on creating roles, see [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) in the *IAM User Guide*\. 

1. Delegate and test access permission from your primary account using IAM roles with any of your other accounts that need to use your 10DLC numbers\. For example, you might delegate access permission from your Production account to your Development account\. For more information about delegating and testing permissions, see [Delegate access across AWS account using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) in the *IAM User Guide*\.

1. Using the new role, send a message using a 10DLC number from the main account\. For more information about using a role, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the IAM User Guide\.