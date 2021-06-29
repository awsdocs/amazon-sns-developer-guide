# Attribute string value matching<a name="string-value-matching"></a>

You can use string values to match message attributes and filter messages\. String values are enclosed in double quotation marks in the JSON policy\.

You can use the following string operations to match message attributes\.

## Exact matching<a name="string-exact-matching"></a>

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

## Prefix matching<a name="string-prefix-matching"></a>

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

## Anything\-but matching<a name="string-anything-but-matching"></a>

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

### Using a prefix with the `anything-but` operator<a name="string-anything-but-matching-prefix"></a>

For attribute *string* matching, you can also use a prefix with the `anything-but` operator\.

For example, the following policy attribute denies the `order-` prefix:

```
"event":[{"anything-but": {"prefix":"order-"}}]
```

It matches either of the following attributes:

```
"event": {"Type": "String", "Value": data-entry}
```

```
"event": {"Type": "String", "Value": order_number}
```

However, it *doesn't* match the following attribute:

```
"event": {"Type": "String", "Value": order-cancelled}
```

## IP address matching<a name="ip-address-matching"></a>

You can use the `cidr` operator to check whether an incoming message originates from a specific IP address or subnet\. 

Consider the following policy attribute:

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

However, it *doesn't* match the following message attribute:

```
"source_ip": {"Type": "String", "Value": "10.1.1.0"}
```