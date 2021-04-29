# 10DLC cross\-account access<a name="sns-settings-sms-crossaccount-10dlc"></a>

10DLC companies and campaigns all reside within a single AWS account\. If you have multiple accounts, you can associate those other accounts with your main account in order to use your 10DLC numbers from any of those accounts\. You do this using the Amazon Pinpoint \(not Amazon SNS\) console\. Then you create an AWS Identity and Access Management \(IAM\) role, and delegate access permissions to your other accounts\.

**To allow your multiple AWS accounts access to your 10DLC numbers**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. Under **Account Settings**, and then under **SMS and voice**, choose the **10DLC** tab\.

1. Register your company and campaign on the main account that will use the 10DLC phone number\. For example, your main account might be a `Production` account, but you want your `Development` account to use a 10DLC number\. 

1. Create the IAM role in your main account, which allows another account to call the `SendMessage` operation of the Pinpoint API\. See [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) in the *IAM User Guide* for more information on creating roles\. 

1. Delegate and test access permission from your main account using IAM roles with any of other your accounts that need to use your 10DLC numbers\. For example, you might delegate access permission from your `Production` account to your `Development` account\. See [Delegate access across AWS accounts using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_cross-account-with-roles.html) in the *IAM User Guide* for more information about delegating and testing\.

1. Using the new role, send a message using a 10DLC number from the main account\. When sending the message, Amazon SNS assumes the main account role and sends the message\. See [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the *IAM User Guide* for more information on using a role\. 
**Important**  
A sent message from the secondary account is treated as if it were sent from your main account\. Quotas and billing are counted against this account and not any secondary accounts\.