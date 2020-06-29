# Amazon SNS subscription filter policies<a name="sns-subscription-filter-policies"></a>

A subscription filter policy allows you to specify attribute names and assign a list of values to each attribute name\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

When Amazon SNS evaluates message attributes against the subscription filter policy, it ignores message attributes that aren't specified in the policy\.

**Important**  
AWS services such as IAM and Amazon SNS use a distributed computing model called eventual consistency\. Additions or changes to a subscription filter policy require up to 15 minutes to fully take effect\. 

A subscription accepts a message under the following conditions:
+ Each attribute name in a filter policy matches an attribute name assigned to the message\.
+ For each matching attribute name, at least one match exists between the following:
  + the values of the attribute name in the filter policy
  + the message attributes

**Topics**
+ [Example message with attributes](#example-message-with-attributes)
+ [Example filter policies](#example-filter-policies)
+ [Filter policy constraints](#subscription-filter-policy-constraints)
+ [Attribute string value matching](#string-value-matching)
+ [Attribute numeric value matching](#numeric-value-matching)
+ [Attribute key matching](#attribute-key-matching)
+ [AND/OR logic](#and-or-logic)

## Example message with attributes<a name="example-message-with-attributes"></a>

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
         "Value":210.75
      }
   }
}
```

For information about applying attributes to a message, see [Amazon SNS message attributes](sns-message-attributes.md)\.

## Example filter policies<a name="example-filter-policies"></a>

The following filter policies accept or reject messages based on their attribute names and values\.

### A policy that accepts messages<a name="policy-accepts-messages"></a>

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

### A policy that rejects messages<a name="policy-rejects-messages"></a>

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

## Filter policy constraints<a name="subscription-filter-policy-constraints"></a>

When you create a filter policy, keep the following constraints in mind:
+ For the `String` data type, the attribute comparison between policy and message is case\-sensitive\.
+ A numeric policy attribute can have a value from \-109 to 109, with 5 digits of accuracy after the decimal point\.
+ The total combination of values must not exceed 150\. Calculate the total combination by multiplying the number of values in each array\.

  Consider the following policy:

  ```
  {
     "key_a": ["value_one", "value_two", "value_three"],
     "key_b": ["value_one"],
     "key_c": ["value_one", "value_two"]
  }
  ```

  The first array has three values, the second has one value, and the third has two values\. The total combination is calculated as follows:

  ```
  3 x 1 x 2 = 6
  ```
+ Amazon SNS compares policy attributes only to message attributes that have the following data types:
  + `String`
  + `String.Array`
  + `Number`
+ Amazon SNS ignores message attributes with the `Binary` data type\.
+ The JSON of the filter policy can contain the following:
  + strings enclosed in quotation marks
  + numbers
  + the keywords `true`, `false`, and `null`, without quotation marks
+ When you use the Amazon SNS API, you must pass the JSON of the filter policy as a valid UTF\-8 string\.
+ A filter policy can have a maximum of 5 attribute names\.
+ The maximum size of a policy is 256 KB\.
+ By default, you can have up to 200 filter policies per AWS account, per region\. To increase this quota, submit a [quota increase request](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\.

## Attribute string value matching<a name="string-value-matching"></a>

You can use string values to match message attributes and filter messages\. String values are enclosed in double quotation marks in the JSON policy\.

You can use the following string operations to match message attributes\.

### Exact matching<a name="string-exact-matching"></a>

Exact matching occurs when a policy attribute value matches one or more message attribute values\.

Consider the following policy attribute:

```
"customer_interests": ["rugby", "tennis"]
```

It matches the following message attributes:

```
"customer_interests": {"Type": "String", "Value": "rugby"}
```

```
"customer_interests": {"Type": "String", "Value": "tennis"}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "baseball"}
```

### Anything\-but matching<a name="string-anything-but-matching"></a>

When a policy attribute value includes the keyword `anything-but`, it matches any message attribute that *doesn't* include any of the policy attribute values\.

Consider the following policy attribute:

```
"customer_interests": [{"anything-but": ["rugby", "tennis"]}]
```

It matches either of the following message attributes:

```
"customer_interests": {"Type": "String", "Value": "baseball"}
```

```
"customer_interests": {"Type": "String", "Value": "football"}
```

It also matches the following message attribute \(because it contains a value that *isn't* `rugby` or `tennis`\):

```
"customer_interests": {"Type": "String.Array", "Value": "[\"rugby\", \"baseball\"]"}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "rugby"}
```

### Prefix matching<a name="string-prefix-matching"></a>

When a policy attribute includes the keyword `prefix`, it matches any message attribute value that begins with the specified characters\.

Consider the following policy attribute:

```
"customer_interests": [{"prefix": "bas"}]
```

It matches either of the following message attributes:

```
"customer_interests": {"Type": "String", "Value": "baseball"}
```

```
"customer_interests": {"Type": "String", "Value": "basketball"}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "rugby"}
```

## Attribute numeric value matching<a name="numeric-value-matching"></a>

You can use numeric values to match message attributes and filter messages\. Numeric values aren't enclosed in double quotation marks in the JSON policy\. You can use the following numeric operations to match message attributes\.

### Exact matching<a name="numeric-exact-matching"></a>

When a policy attribute value includes the keyword `numeric` and the operator `=`, it matches any message attribute that has the same name and an equal numeric value\.

Consider the following policy attribute:

```
"price_usd": [{"numeric": ["=",301.5]}]
```

It matches either of the following message attributes:

```
"price_usd": {"Type": "Number", "Value": 301.5}
```

```
"price_usd": {"Type": "Number", "Value": 3.015e2}
```

### Anything\-but matching<a name="numeric-anything-but-matching"></a>

When a policy attribute value includes the keyword `anything-but`, it matches any message attribute that *doesn't* include any of the policy attribute values\.

Consider the following policy attribute:

```
"price": [{"anything-but": [100, 500]}]
```

It matches either of the following message attributes:

```
"price": {"Type": "Number", "Value": 101}
```

```
"price": {"Type": "Number", "Value": 100.1}
```

It also matches the following message attribute \(because it contains a value that *isn't* `100` or `500`\):

```
"price": {"Type": "Number.Array", "Value": "[100, 50]"}
```

However, it doesn't match the following message attribute:

```
"price": {"Type": "Number", "Value": 100}
```

### Value range matching<a name="numeric-value-range-matching"></a>

In addition to the operator `=`, a numeric policy attribute can include the following operators: `<`, `<=`, `>`, and `>=`\.

Consider the following policy attribute:

```
"price_usd": [{"numeric": ["<", 0]}]
```

It matches any message attributes with negative numeric values\.

Consider another message attribute:

```
"price_usd": [{"numeric": [">", 0, "<=", 150]}]
```

It matches any message attributes with positive numbers up to and including 150\.

## Attribute key matching<a name="attribute-key-matching"></a>

You can use the `exists` operator to check whether an incoming message has an attribute whose key is listed in the filter policy\.

Consider the following policy attribute:

```
"store": [{"exists": true}]
```

It matches any message that contains the `store` attribute key, such as the following:

```
"store": "fans"
"customer_interests": ["baseball", "basketball"]
```

However, it doesn't match any message *without* the `store` attribute key, such as the following:

```
"customer_interests": ["baseball", "basketball"]
```

Note: You cannot use the `exists` operator to match messages in which an attribute does not exist\. Filtering will NOT match any messages if you set `[{"exists": false}]`\. 

## AND/OR logic<a name="and-or-logic"></a>

You can use operations that include AND/OR logic to match message attributes\.

### AND logic<a name="and-logic"></a>

You can apply AND logic using multiple attribute names\.

Consider the following policy:

```
{
  "customer_interests": ["rugby"],
  "price_usd": [{"numeric": [">", 100]}]
}
```

It matches any message attributes with the value of `customer_interests` set to `rugby` *and* the value of `price_usd` set to a number larger than 100\.

### OR logic<a name="or-logic"></a>

You can apply OR logic by assigning multiple values to an attribute name\.

Consider the following policy attribute:

```
"customer_interests": ["rugby", "football", "baseball"]
```

It matches any message attributes with the value of `customer_interests` set to `rugby`, `football`, *or* `baseball`\.