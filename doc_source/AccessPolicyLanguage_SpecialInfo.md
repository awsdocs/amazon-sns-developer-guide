# Special Information for Amazon SNS Policies<a name="AccessPolicyLanguage_SpecialInfo"></a>

The following list gives information specific to the Amazon SNS implementation of access control:
+ Each policy must cover only a single topic \(when writing a policy, don't include statements that cover different topics\)
+ Each policy must have a unique policy `Id`
+ Each statement in a policy must have a unique statement `sid`

## Amazon SNS Policy Limits<a name="w3aac28b7c11b7"></a>

The following table lists the maximum limits for policy information\.


| Name | Maximum Limit | 
| --- | --- | 
|  Bytes  |  30 kb  | 
|  Statements  |  100  | 
|  Principals  |  1 to 200 \(0 is invalid\.\)  | 
|  Resource  |  1 \(0 is invalid\. The value must match the ARN of the policy's topic\.\)  | 

## Valid Amazon SNS Policy Actions<a name="sns_aspen_actions"></a>

Amazon SNS supports the actions shown in the following table\.


| Action | Description | 
| --- | --- | 
|  sns:AddPermission  | Grants permission to add permissions to the topic policy\. | 
|  sns:DeleteTopic  | Grants permission to delete a topic\. | 
|  sns:GetTopicAttributes  | Grants permission to receive all of the topic attributes\. | 
|  sns:ListSubscriptionsByTopic  | Grants permission to retrieve all the subscriptions to a specific topic\. | 
|  sns:Publish  | Grants permission to publish to a topic or endpoint\. For more information, see [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) in the Amazon Simple Notification Service API Reference  | 
|  sns:RemovePermission  | Grants permission to remove any permissions in the topic policy\. | 
|  sns:SetTopicAttributes  | Grants permission to set a topic's attributes\. | 
|  sns:Subscribe  | Grants permission to subscribe to a topic\. | 

## Amazon SNS Keys<a name="sns_aspen_keys"></a>

Amazon SNS uses the following service\-specific keys\. You can use these in policies that restrict access to `Subscribe` requests\.
+ **sns:Endpoint—**The URL, email address, or ARN from a `Subscribe` request or a previously confirmed subscription\. Use with string conditions \(see [Example Policies for Amazon SNS](sns-using-identity-based-policies.md#ExamplePolicies_SNS)\) to restrict access to specific endpoints \(e\.g\., \*@example\.com\)\.
+ **sns:Protocol—**The `protocol` value from a `Subscribe` request or a previously confirmed subscription\. Use with string conditions \(see [Example Policies for Amazon SNS](sns-using-identity-based-policies.md#ExamplePolicies_SNS)\) to restrict publication to specific delivery protocols \(e\.g\., https\)\.

**Important**  
When you use a policy to control access by sns:Endpoint, be aware that DNS issues might affect the endpoint's name resolution in the future\.