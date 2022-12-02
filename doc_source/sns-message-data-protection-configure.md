# Creating data protection policies<a name="sns-message-data-protection-configure"></a>

[Data protection policies](sns-message-data-protection-policies.md) help you safeguard the data that's published to your Amazon SNS topics by auditing, de\-identifying \(masking or redacting\), and denying \(blocking\) sensitive information that moves between applications or AWS services\. You can use AWS API, AWS CLI, AWS CloudFormation, or AWS Management Console to create data protection policies in Amazon SNS\. Only one policy can be defined per Amazon SNS topic\. Each data protection policy can have one or more de\-identify and deny statements, but only one audit statement\.

**Topics**
+ [Creating data protection policies to secure message data \(API\)](sns-message-data-protection-configure-api.md)
+ [Creating data protection policies to secure message data \(CLI\)](sns-message-data-protection-configure-cli.md)
+ [Creating data protection policies to secure message data \(CloudFormation\)](sns-message-data-protection-configure-cfn.md)
+ [Creating data protection policies to secure message data \(Console\)](sns-message-data-protection-configure-console.md)
+ [Creating data protection policies to secure message data \(SDK\)](sns-message-data-protection-configure-sdk.md)