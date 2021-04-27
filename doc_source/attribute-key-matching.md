# Attribute key matching<a name="attribute-key-matching"></a>

You can use the `exists` operator to return incoming messages with or without specified attributes in the filter policy:
+ Use `"exists": true` to return incoming messages that include the specified attribute\.

  For example, the following attribute uses the `exists` operator with a value of `true`:

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
+ Use `"exists": false` to return incoming messages that *don't* include the specified attribute\.

  The following example shows the effect of using the `exists` operator with a value of `false`:

  ```
  "store": [{"exists": false}]
  ```

  It *doesn't* match any message that contains the `store` attribute key, such as the following:

  ```
  "store": "fans" 
  "customer_interests": ["baseball", "basketball"]
  ```

  However, it matches any messages *without* the `store` attribute key, such as the following:

  ```
  "customer_interests": ["baseball", "basketball"]
  ```