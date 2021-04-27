# Filter policy constraints<a name="subscription-filter-policy-constraints"></a>

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