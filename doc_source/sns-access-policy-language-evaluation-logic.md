# Evaluation logic<a name="sns-access-policy-language-evaluation-logic"></a>

The goal at evaluation time is to decide whether a grant request should be allowed or denied\. The evaluation logic follows several basic rules:
+ By default, all requests to use your resource coming from anyone but you are denied
+ An allow overrides any default denies
+ An explicit deny overrides any allows
+ The order in which the policies are evaluated is not important

The following flow chart and discussion describe in more detail how the decision is made\.

![\[Evaluation flow chart\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Evaluation_Flow.gif)


|  |  | 
| --- |--- |
| 1 |  The decision starts with a default deny\.  | 
| 2 |   The enforcement code then evaluates all the policies that are applicable to the request \(based on the resource, principal, action, and conditions\)\.  The order in which the enforcement code evaluates the policies is not important\.  | 
| 3 |   In all those policies, the enforcement code looks for an explicit deny instruction that would apply to the request\. If it finds even one, the enforcement code returns a decision of "deny" and the process is finished \(this is an explicit deny; for more information, see [Explicit deny](sns-access-policy-language-key-concepts.md#Define_HardDeny)\)\.  | 
| 4 |  If no explicit deny is found, the enforcement code looks for any "allow" instructions that would apply to the request\. If it finds even one, the enforcement code returns a decision of "allow" and the process is done \(the service continues to process the request\)\.   | 
| 5 |  If no allow is found, then the final decision is "deny" \(because there was no explicit deny or allow, this is considered a *default deny* \(for more information, see [Default deny](sns-access-policy-language-key-concepts.md#Define_SoftDeny)\)\.  | 

## The interplay of explicit and default denials<a name="denials"></a>

A policy results in a default deny if it doesn't directly apply to the request\. For example, if a user requests to use Amazon SNS, but the policy on the topic doesn't refer to the user's AWS account at all, then that policy results in a default deny\.

A policy also results in a default deny if a condition in a statement isn't met\. If all conditions in the statement are met, then the policy results in either an allow or an explicit deny, based on the value of the Effect element in the policy\. Policies don't specify what to do if a condition isn't met, and so the default result in that case is a default deny\.

For example, let's say you want to prevent requests coming in from Antarctica\. You write a policy \(called Policy A1\) that allows a request only if it doesn't come from Antarctica\. The following diagram illustrates the policy\.

![\[Policy allowing request if it's not from Antarctica\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Allow_Override_1.gif)

If someone sends a request from the U\.S\., the condition is met \(the request is not from Antarctica\)\. Therefore, the request is allowed\. But, if someone sends a request from Antarctica, the condition isn't met, and the policy's result is therefore a default deny\. 

You could turn the result into an explicit deny by rewriting the policy \(named Policy A2\) as in the following diagram\. Here, the policy explicitly denies a request if it comes from Antarctica\.

![\[Policy denying request if it's from Antarctica\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Allow_Override_2.gif)

If someone sends a request from Antarctica, the condition is met, and the policy's result is therefore an explicit deny\.

The distinction between a default deny and an explicit deny is important because a default deny can be overridden by an allow, but an explicit deny can't\. For example, let's say there's another policy that allows requests if they arrive on June 1, 2010\. How does this policy affect the overall outcome when coupled with the policy restricting access from Antarctica? We'll compare the overall outcome when coupling the date\-based policy \(we'll call Policy B\) with the preceding policies A1 and A2\. Scenario 1 couples Policy A1 with Policy B, and Scenario 2 couples Policy A2 with Policy B\. The following figure and discussion show the results when a request comes in from Antarctica on June 1, 2010\.

![\[Override of default deny\]](http://docs.aws.amazon.com/sns/latest/dg/images/AccessPolicyLanguage_Allow_Override.gif)

In Scenario 1, Policy A1 returns a default deny, as described earlier in this section\. Policy B returns an allow because the policy \(by definition\) allows requests that come in on June 1, 2010\. The allow from Policy B overrides the default deny from Policy A1, and the request is therefore allowed\.

In Scenario 2, Policy A2 returns an explicit deny, as described earlier in this section\. Again, Policy B returns an allow\. The explicit deny from Policy A2 overrides the allow from Policy B, and the request is therefore denied\.