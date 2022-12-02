# Message filtering for FIFO topics<a name="fifo-message-filtering"></a>

Amazon SNS FIFO topics support message filtering\. Using message filtering simplifies your architecture by offloading the message routing logic from your publisher systems and the message filtering logic from your subscriber systems\.

When you subscribe an Amazon SQS FIFO queue to an SNS FIFO topic, you can use message filtering to specify that the subscriber receives a subset of messages, rather than all of them\. Each subscriber can set its own filter policy as subscription attributes\. Based on the filter policy scope, filter policy is matched against incoming message\-attributes or message\-body\. If the filter policy matches, the topic delivers a copy of the message to the subscriber\. If there's no match, the topic doesn't deliver a copy of the message\.

In the [auto parts price management example use case](fifo-example-use-case.md), assume that the following Amazon SNS filter policies are set and filter policy scope is `MessageBody`:
+ For the wholesale queue, the filter policy `{"business":["wholesale"]}` matches every message which contains a key named `business` and with `wholesale` in the set of values\. In the following diagram, one of the key in message **m1** is `business` with the value of `wholesale`\. One of the key in message **m3** is `business` with a value of `["wholesale,retail"]`\. Thus, both **m1** and **m3** match the filter policy's criteria, and both messages are delivered to the wholesale queue\.
+ For the retail queue, the filter policy `{"business":["retail"]}` matches every message which contains a key named `business` and with `retail` in the set of values\. In the diagram, one of the key in message **m2** is `business` with the value of `retail`\. One of the key in message **m3** is `business` with the value of `["wholesale,retail"]`\. Thus, both **m2** and **m3** match the filter policy's criteria, and both messages are delivered to the retail queue\.

The following diagram shows the effect of messaging filtering using these filter policies\.

![\[Message filtering for SNS FIFO topics.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-fifo-filtering.png)

SNS FIFO topics support a variety of matching operators, including attribute string values, attribute numeric values, and attribute keys\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

SNS FIFO topics don't deliver duplicate messages to subscribed endpoints\. For more information, see [Message deduplication for FIFO topics](fifo-message-dedup.md)\.