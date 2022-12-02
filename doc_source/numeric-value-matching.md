# Numeric value matching<a name="numeric-value-matching"></a>

You can filter messages by matching numeric values to message attribute values or to message body property values\. Numeric values aren't enclosed in double quotation marks in the JSON policy\. You can use the following numeric operations for filtering\.

**Note**  
Prefixes are supported for *string* matching only\.

## Exact matching<a name="numeric-exact-matching"></a>

When a policy property value includes the keyword `numeric` and the operator `=`, it matches any message attribute or message body property values that have the same name and an equal numeric value\.

Consider the following policy property:

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

It also matches either of the following message bodies:

```
{
   "price_usd": 301.5
}
```

```
{
   "price_usd": 3.015e2
}
```

## Anything\-but matching<a name="numeric-anything-but-matching"></a>

When a policy property value includes the keyword `anything-but`, it matches any message attribute or message body property values that *don't* include any of the policy property values\.

Consider the following policy property:

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

It also matches either of the following message bodies:

```
{
   "price": 101
}
```

```
{
   "price": 100.1
}
```

Moreover, it matches the following message attribute \(because it contains a value that *isn't* `100` or `500`\):

```
"price": {"Type": "Number.Array", "Value": "[100, 50]"}
```

And it also matches the following message body \(because it contains a value that *isn't* `100` or `500`\):

```
{
   "price": [100, 50]
}
```

However, it doesn't match the following message attribute:

```
"price": {"Type": "Number", "Value": 100}
```

Nor does it match the following message body:

```
{
   "price": 100
}
```

## Value range matching<a name="numeric-value-range-matching"></a>

In addition to the operator `=`, a numeric policy property can include the following operators: `<`, `<=`, `>`, and `>=`\.

Consider the following policy property:

```
"price_usd": [{"numeric": ["<", 0]}]
```

It matches any message attribute or message body property with negative numeric values\.

Consider another message attribute:

```
"price_usd": [{"numeric": [">", 0, "<=", 150]}]
```

It matches any message attribute or message body property with positive numbers up to and including 150\.