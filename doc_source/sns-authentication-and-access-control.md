# Authentication and Access Control for Amazon SNS<a name="sns-authentication-and-access-control"></a>

**Topics**
+ [Overview of Managing Access Permissions to Your Amazon Simple Notification Service Resource](sns-overview-of-managing-access.md)
+ [Special Information for Amazon SNS Policies](AccessPolicyLanguage_SpecialInfo.md)
+ [Controlling User Access to Your AWS Account](sns-using-identity-based-policies.md)

[Amazon SNS](https://aws.amazon.com/sns/) supports other protocols beside email\. You can use HTTP, HTTPS, and Amazon SQS queues\. You have detailed control over which endpoints a topic allows, who is able to publish to a topic, and under what conditions\. This appendix shows you how to control through the use of *access control policies*\. 

The main portion of this section includes basic concepts you need to understand, how to write a policy, and the logic Amazon Web Services \(AWS\) uses to evaluate policies and decide whether to grant the requester access to the resource\. Although most of the information in this section is service\-agnostic, there are some Amazon SNS\-specific details you need to know\. For more information, see [Special Information for Amazon SNS Policies](AccessPolicyLanguage_SpecialInfo.md)\.