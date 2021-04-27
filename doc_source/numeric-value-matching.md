# Attribute numeric value matching<a name="numeric-value-matching"></a>

You can use numeric values to match message attributes and filter messages\. Numeric values aren't enclosed in double quotation marks in the JSON policy\. You can use the following numeric operations to match message attributes\. 

**Note**  
Prefixes are supported for attribute *string* matching only\.

## Exact matching<a name="numeric-exact-matching"></a>

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

## Anything\-but matching<a name="numeric-anything-but-matching"></a>

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

## Value range matching<a name="numeric-value-range-matching"></a>

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