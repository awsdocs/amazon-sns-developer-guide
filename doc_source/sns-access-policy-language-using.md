# Using the Access Policy Language<a name="sns-access-policy-language-using"></a>

The following figure and table describe the general process of how access control works with the access policy language\. 

![\[Basic flow for access control with the access policy language\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Basic_Flow.gif)


**Process for using access control with the Access Policy Language**  

|  |  | 
| --- |--- |
|  1  |  You write a policy for your resource\. For example, you write a policy to specify permissions for your Amazon SNS topics\.  | 
|  2  |  You upload your policy to AWS\. The AWS service itself provides an API you use to upload your policies\. For example, you use the Amazon SNS `SetTopicAttributes` action to upload a policy for a particular Amazon SNS topic\.  | 
|  3  |  Someone sends a request to use your resource\. For example, a user sends a request to Amazon SNS to use one of your topics\.   | 
|  4  |  The AWS service determines which policies are applicable to the request\. For example, Amazon SNS looks at all the available Amazon SNS policies and determines which ones are applicable \(based on what the resource is, who the requester is, etc\.\)\.  | 
|  5  |  The AWS service evaluates the policies\. For example, Amazon SNS evaluates the policies and determines if the requester is allowed to use your topic or not\. For information about the decision logic, see [Evaluation logic](sns-access-policy-language-evaluation-logic.md)\.  | 
|  6  |  The AWS service either denies the request or continues to process it\.  For example, based on the policy evaluation result, the service either returns an "Access denied" error to the requester or continues to process the request\.  | 