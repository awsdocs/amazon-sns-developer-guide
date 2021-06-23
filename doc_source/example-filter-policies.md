# Example filter policies<a name="example-filter-policies"></a>

The following example shows a message payload sent by an Amazon SNS topic that publishes customer transactions\. The `MessageAttributes` field includes attributes that describe the transaction:
+ Customer's interests
+ Store name
+ Event state
+ Purchase price in USD

Because this message includes the `MessageAttributes` field, any topic subscription that includes a filter policy can selectively accept or reject the message\.

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

For information about applying attributes to a message, see [Amazon SNS message attributes](sns-message-attributes.md)\.

The following filter policies accept or reject messages based on their attribute names and values\.

## A policy that accepts messages<a name="policy-accepts-messages"></a>

The attributes in the following subscription filter policy match the attributes assigned to the example message\.

If any single attribute in this policy doesn't match an attribute assigned to the message, the policy rejects the message\.

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

## A policy that rejects messages<a name="policy-rejects-messages"></a>

The following subscription filter policy has multiple mismatches between its attributes and the attributes assigned to the example message\. Because the `encrypted` attribute name isn't present in the message attributes, this policy attribute causes the message to be rejected regardless of the value assigned to it\.

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