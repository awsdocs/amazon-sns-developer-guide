# Amazon SNS subscription filter policies<a name="sns-subscription-filter-policies"></a>

A subscription filter policy allows you to specify property names and assign a list of values to each property name\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

When Amazon SNS evaluates message attributes or message body properties against the subscription filter policy, it ignores the ones that aren't specified in the policy\.

**Important**  
AWS services such as IAM and Amazon SNS use a distributed computing model called eventual consistency\. Additions or changes to a subscription filter policy require up to 15 minutes to fully take effect\. 

A subscription accepts a message under the following conditions:
+ When the filter policy scope is set to `MessageAttributes`, each property name in the filter policy matches a message attribute name\. For each matching property name in the filter policy, at least one property value matches the message attribute value\.
+ When the filter policy scope is set to `MessageBody`, each property name in the filter policy matches a message body property name\. For each matching property name in the filter policy, at least one property value matches the message body property value\.

**Topics**
+ [Example filter policies](example-filter-policies.md)
+ [Filter policy constraints](subscription-filter-policy-constraints.md)
+ [String value matching](string-value-matching.md)
+ [Numeric value matching](numeric-value-matching.md)
+ [Key matching](attribute-key-matching.md)
+ [AND/OR logic](and-or-logic.md)