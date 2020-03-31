# Architectural overview<a name="sns-access-policy-language-architectural-overview"></a>

The following figure and table describe the main components that interact to provide access control for your resources\.

![\[Architectural Overview\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Arch_Overview.gif)


|  |  | 
| --- |--- |
| 1 |  You, the resource owner\.  | 
| 2 |  Your resources \(contained within the AWS service; for example, Amazon SQS queues\)\.  | 
| 3 |  Your policies\. Typically you have one policy per resource, although you could have multiple\. The AWS service itself provides an API you use to upload and manage your policies\.  | 
| 4 |  Requesters and their incoming requests to the AWS service\.  | 
| 5 |  The access policy language evaluation code\. This is the set of code within the AWS service that evaluates incoming requests against the applicable policies and determines whether the requester is allowed access to the resource\. For information about how the service makes the decision, see [Evaluation logic](sns-access-policy-language-evaluation-logic.md)\.  | 