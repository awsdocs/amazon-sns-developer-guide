# Amazon SNS subscription filter policies<a name="sns-subscription-filter-policies"></a>

A subscription filter policy allows you to specify attribute names and assign a list of values to each attribute name\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

When Amazon SNS evaluates message attributes against the subscription filter policy, it ignores message attributes that aren't specified in the policy\.

**Important**  
AWS services such as IAM and Amazon SNS use a distributed computing model called eventual consistency\. Additions or changes to a subscription filter policy require up to 15 minutes to fully take effect\. 

A subscription accepts a message under the following conditions:
+ Each attribute name in a filter policy matches an attribute name assigned to the message\.
+ For each matching attribute name, at least one match exists between the following:
  + The values of the attribute name in the filter policy
  + The message attributes

**Topics**
+ [Example filter policies](example-filter-policies.md)
+ [Filter policy constraints](subscription-filter-policy-constraints.md)
+ [Attribute string value matching](string-value-matching.md)
+ [Attribute numeric value matching](numeric-value-matching.md)
+ [Attribute key matching](attribute-key-matching.md)
+ [AND/OR logic](and-or-logic.md)