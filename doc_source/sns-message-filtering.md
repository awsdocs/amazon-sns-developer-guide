# Amazon SNS message filtering<a name="sns-message-filtering"></a>

By default, an Amazon SNS topic subscriber receives every message that's published to the topic\. To receive only a subset of the messages, a subscriber must assign a *filter policy* to the topic subscription\.

A filter policy is a JSON object containing properties that define which messages the subscriber receives\. Amazon SNS supports policies that act on the message attributes or on the message body, according to the filter policy scope that you set for the subscription\. Filter policies for the message body assume that the message payload is a well\-formed JSON object\.

If a subscription doesn't have a filter policy, the subscriber receives every message published to its topic\. When you publish a message to a topic with a filter policy in place, Amazon SNS compares the message attributes or the message body to the properties in the filter policy for each of the topic's subscriptions\. If any of the message attributes or message body properties match, Amazon SNS sends the message to the subscriber\. Otherwise, Amazon SNS doesn't send the message to that subscriber\. 

For more information, see [Filter Messages Published to Topics](https://aws.amazon.com/getting-started/tutorials/filter-messages-published-to-topics/)\.

**Topics**
+ [Amazon SNS subscription filter policy scope](#sns-message-filtering-scope)
+ [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md)
+ [Applying a subscription filter policy](message-filtering-apply.md)
+ [Removing a subscription filter policy](message-filtering-policy-remove.md)

## Amazon SNS subscription filter policy scope<a name="sns-message-filtering-scope"></a>

The `FilterPolicyScope` subscription attribute lets you choose the filtering scope by setting one of the following values:
+ `MessageAttributes` – The filter policy is applied to the message attributes\. This is the default\.
+ `MessageBody` – The filter policy is applied to the message body\.

**Note**  
If no filter policy scope is defined for an existing filter policy, the scope defaults to `MessageAttributes`\.