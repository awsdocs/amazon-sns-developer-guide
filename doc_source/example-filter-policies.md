# Example filter policies<a name="example-filter-policies"></a>

The following example shows a message payload delivered by an Amazon SNS topic that processes customer transactions\.

The first example includes the `MessageAttributes` field with attributes that describe the transaction:
+ Customer's interests
+ Store name
+ Event state
+ Purchase price in USD

Because this message includes the `MessageAttributes` field, any topic subscription that sets a `FilterPolicy` can selectively accept or reject the message, as long as `FilterPolicyScope` is set to `MessageAttributes` in the subscription\. For information about applying attributes to a message, see [Amazon SNS message attributes](sns-message-attributes.md)\.

```
{
   "Type": "Notification",
   "MessageId": "a1b2c34d-567e-8f90-g1h2-i345j67klmn8",
   "TopicArn": "arn:aws:sns:us-east-2:123456789012:MyTopic",
   "Message": "message-body-with-transaction-details",
   "Timestamp": "2019-11-03T23:28:01.631Z",
   "SignatureVersion": "4",
   "Signature": "signature",
   "UnsubscribeURL": "unsubscribe-url",
   "MessageAttributes": {
      "customer_interests": {
         "Type": "String.Array",
         "Value": "[\"soccer\", \"rugby\", \"hockey\"]"
      },
      "store": {
         "Type": "String",
         "Value":"example_corp"
      },
      "event": {
         "Type": "String",
         "Value": "order_placed"
      },
      "price_usd": {
         "Type": "Number", 
         "Value": "210.75"
      }
   }
}
```

The following example shows the same attributes included within the `Message` field, also referred to as the *message payload* or *message body*\. Any topic subscription that includes a `FilterPolicy` can selectively accept or reject the message, as long as `FilterPolicyScope` is set to `MessageBody` in the subscription\. 

```
{
"Type": "Notification",
   "MessageId": "a1b2c34d-567e-8f90-g1h2-i345j67klmn8",
   "TopicArn": "arn:aws:sns:us-east-2:123456789012:MyTopic",
   "Message": "{
      \"customer_interests\": [\"soccer\", \"rugby\", \"hockey\"],
      \"store\": \"example_corp\",
      \"event\":\"order_placed\",
      \"price_usd\":210.75
   }",
   "Timestamp": "2019-11-03T23:28:01.631Z",
   "SignatureVersion": "4",
   "Signature": "signature",
   "UnsubscribeURL": "unsubscribe-url"
}
```

The following filter policies accept or reject messages based on their property names and values\.

## A policy that accepts the example message<a name="policy-accepts-messages"></a>

The properties in the following subscription filter policy match the attributes assigned to the example message\. Note that the same filter policy works for a `FilterPolicyScope` whether it's set to `MessageAttributes` or `MessageBody`\. Each subscriber chooses their filtering scope according to the composition of the messages that they receive from the topic\.

If any single property in this policy doesn't match an attribute assigned to the message, the policy rejects the message\.

```
{
   "store": ["example_corp"],
   "event": [{"anything-but": "order_cancelled"}],
   "customer_interests": [
      "rugby",
      "football",
      "baseball"
   ],
   "price_usd": [{"numeric": [">=", 100]}]
}
```

## A policy that rejects the example message<a name="policy-rejects-messages"></a>

The following subscription filter policy has multiple mismatches between its properties and the attributes assigned to the example message\. For example, because the `encrypted` property name isn't present in the message attributes, this policy property causes the message to be rejected regardless of the value assigned to it\. 

If any mismatches occur, the policy rejects the message\.

```
{
   "store": ["example_corp"],
   "event": ["order_cancelled"],
   "encrypted": [false],
   "customer_interests": [
      "basketball",
      "baseball"
   ]
}
```