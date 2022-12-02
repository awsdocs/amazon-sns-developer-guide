# String value matching<a name="string-value-matching"></a>

You can filter messages by matching string values to message attribute values, or to message body property values\. String values are enclosed in double quotation marks in the JSON policy\.

You can use the following string operations to match message attributes or message body\.

## Exact matching<a name="string-exact-matching"></a>

Exact matching occurs when a policy property value matches one or more message attribute values\.

Consider the following policy property:

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

It also matches the following message bodies:

```
{
   "customer_interests": "rugby"
}
```

```
{
   "customer_interests": "tennis"
}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "baseball"}
```

Nor does it match the following message body:

```
{
   "customer_interests": "baseball"
}
```

## Prefix matching<a name="string-prefix-matching"></a>

When a policy property includes the keyword `prefix`, it matches any message attribute or body property values that begin with the specified characters\.

Consider the following policy property:

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

It also matches either of the following message bodies:

```
{
   "customer_interests": "baseball"
}
```

```
{
   "customer_interests": "basketball"
}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "rugby"}
```

Nor does it match the following message body:

```
{
   "customer_interests": "rugby"
}
```

## Anything\-but matching<a name="string-anything-but-matching"></a>

When a policy property value includes the keyword `anything-but`, it matches any message attribute or message body values that *don't* include any of the policy property values\.

Consider the following policy property:

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

It also matches either of the following message bodies:

```
{
   "customer_interests": "baseball"
}
```

```
{
   "customer_interests": "football"
}
```

Moreover, it matches the following message attribute \(because it contains a value that *isn't* `rugby` or `tennis`\):

```
"customer_interests": {"Type": "String.Array", "Value": "[\"rugby\", \"baseball\"]"}
```

And it also matches the following message body \(because it contains a value that isn't `rugby` or `tennis`\):

```
{
   "customer_interests": ["rugby", "baseball"]
}
```

However, it doesn't match the following message attribute:

```
"customer_interests": {"Type": "String", "Value": "rugby"}
```

Nor does it match the following message body:

```
{
   "customer_interests": ["rugby"]
}
```

### Using a prefix with the `anything-but` operator<a name="string-anything-but-matching-prefix"></a>

For *string* matching, you can also use a prefix with the `anything-but` operator\.

For example, the following policy property denies the `order-` prefix:

```
"event":[{"anything-but": {"prefix": "order-"}}]
```

It matches either of the following attributes:

```
"event": {"Type": "String", "Value": "data-entry"}
```

```
"event": {"Type": "String", "Value": "order_number"}
```

It also matches either of the following message bodies:

```
{
   "event": "data-entry"
}
```

```
{
   "event": "order_number"
}
```

However, it *doesn't* match the following message attribute:

```
"event": {"Type": "String", "Value": "order-cancelled"}
```

Nor does it match the following message body:

```
{
   "event": "order-cancelled"
}
```

## IP address matching<a name="ip-address-matching"></a>

You can use the `cidr` operator to check whether an incoming message originates from a specific IP address or subnet\. 

Consider the following policy property:

```
"source_ip":[{"cidr": "10.0.0.0/24"}]
```

It matches either of the following message attributes:

```
"source_ip": {"Type": "String", "Value": "10.0.0.0"}
```

```
"source_ip": {"Type": "String", "Value": "10.0.0.255"}
```

It also matches either of the following message bodies:

```
{
   "source_ip": "10.0.0.0"
}
```

```
{
   "source_ip": "10.0.0.255"
}
```

However, it *doesn't* match the following message attribute:

```
"source_ip": {"Type": "String", "Value": "10.1.1.0"}
```

Nor does it match the following message body:

```
{
   "source_ip": "10.1.1.0"
}
```