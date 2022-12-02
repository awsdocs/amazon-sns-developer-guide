# AND/OR logic<a name="and-or-logic"></a>

You can use operations that include AND/OR logic to match message attributes or message body properties\.

## AND logic<a name="and-logic"></a>

You can apply AND logic using multiple property names\.

Consider the following policy:

```
{
  "customer_interests": ["rugby"],
  "price_usd": [{"numeric": [">", 100]}]
}
```

It matches any message attribute or message body property with the value of `customer_interests` set to `rugby` *and* the value of `price_usd` set to a number larger than 100\.

## OR logic<a name="or-logic"></a>

You can apply OR logic by assigning multiple values to a property name\.

Consider the following policy:

```
{
   "customer_interests": ["rugby", "football", "baseball"]
}
```

It matches any message attribute or message body property with the value of `customer_interests` set to `rugby`, `football`, *or* `baseball`\.

**Note**  
Currently, you can't use SNS filters to apply `OR` logic across different message attributes\. Instead, you can use multiple SNS subscriptions with different endpoints to achieve the same effect\. For example, assume that you have message attributes named `customer_interests` and `customer_preferences`\. To apply `OR` logic across both attributes, create an SNS subscription to match each message attribute\. Then, you can use your subscriber application to consume both types of messages through the different endpoints\. 