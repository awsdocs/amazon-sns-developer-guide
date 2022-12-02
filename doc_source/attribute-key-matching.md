# Key matching<a name="attribute-key-matching"></a>

You can use the `exists` operator to match incoming messages with or without specified properties in the filter policy:
+ Use `"exists": true` to match incoming messages that include the specified property\.

  For example, the following policy property uses the `exists` operator with a value of `true`:

  ```
  "store": [{"exists": true}]
  ```

  It matches any list of message attributes that contains the `store` attribute key, such as the following:

  ```
  "store": {"Type": "String", "Value": "fans"}
  "customer_interests": {"Type": "String.Array", "Value": "[\"baseball\", \"basketball\"]"}
  ```

  It also matches either of the following message body:

  ```
  {
      "store": "fans"
      "customer_interests": ["baseball", "basketball"]
  }
  ```

  However, it doesn't match any list of message attributes *without* the `store` attribute key, such as the following:

  ```
  "customer_interests": {"Type": "String.Array", "Value": "[\"baseball\", \"basketball\"]"}
  ```

  Nor does it match the following message body:

  ```
  {
      "customer_interests": ["baseball", "basketball"]
  }
  ```
+ Use `"exists": false` to match incoming messages that *don't* include the specified property\.

  For example, the following policy property uses the `exists` operator with a value of `false`:

  ```
  "store": [{"exists": false}]
  ```

  It *doesn't* match any list of message attributes that contains the `store` attribute key, such as the following:

  ```
  "store": {"Type": "String", "Value": "fans"}
  "customer_interests": {"Type": "String.Array", "Value": "[\"baseball\", \"basketball\"]"}
  ```

  It also doesn’t match the following message body:

  ```
  {
      "store": "fans"
      "customer_interests": ["baseball", "basketball"]
  }
  ```

  However, it matches any list of message attributes *without* the `store` attribute key, such as the following:

  ```
  "customer_interests": {"Type": "String.Array", "Value": "[\"baseball\", \"basketball\"]"}
  ```

  It also matches the following message body:

  ```
  "{
      "customer_interests": ["baseball", "basketball"]
  }
  ```