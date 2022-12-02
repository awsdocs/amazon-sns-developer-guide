# Filter policy constraints<a name="subscription-filter-policy-constraints"></a>

When you create a filter policy, keep the following constraints in mind\.

## Common policy constraints<a name="subscription-filter-policy-common-constraints"></a>
+ For string matching, the comparison is case\-sensitive\.
+ For numeric matching, the value can range from \-109 to 109, with five digits of accuracy after the decimal point\.
+ For the complexity of the filter policy, the total combination of values must not exceed 150\. Calculate the total combination by multiplying the number of values in each array\.

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
+ The JSON of the filter policy can contain the following:
  + Strings enclosed in quotation marks
  + Numbers
  + The keywords `true`, `false`, and `null`, without quotation marks
+ When you use the Amazon SNS API, you must pass the JSON of the filter policy as a valid UTF\-8 string\.
+ The maximum size of a policy is 256 KB\.
+ By default, you can have up to 200 filter policies per topic, and 10,000 filter policies per AWS account\. To increase this quota, you can use [AWS Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html)\.

## Policy constraints for attribute\-based filtering<a name="subscription-filter-policy-attribute-constraints"></a>
+ Attribute\-based filtering is the default option\. `FilterPolicyScope` is set to `MessageAttributes` in the subscription\.
+ Amazon SNS doesn't accept a nested filter policy for attribute\-based filtering\.
+ Amazon SNS compares policy properties only to message attributes that have the following data types:
  + `String`
  + `String.Array`
  + `Number`
+ Amazon SNS ignores message attributes with the `Binary` data type\.
+ A filter policy can have a maximum of five attribute names\.

## Policy constraints for payload\-based filtering<a name="subscription-filter-policy-payload-constraints"></a>
+ Amazon SNS accepts a nested filter policy for payload\-based filtering\. Calculate the total combination by multiplying the number of nested keys with the values in each array\.

  Consider the following policy:

  ```
  {
  "key_a": {
      "key_b": {
          "key_c": ["value_one", "value_two", "value_three", "value_four"]
          }
      },
  "key_d": {
      "key_e": ["value_one", "value_two", "value_three"]
      }
  }
  ```
+ The first array has four values in a three\-level nested key, and the second has three values in a two\-level nested key\. The total combination is calculated as follows:

  ```
  3 x 4 x 2 x 3 = 72
  ```
+ A filter policy can have a maximum of five attribute names\. For a nested policy, only parent keys are counted\.
+ To switch from attribute\-based \(default\) to payload\-based filtering, you must set the `FilterPolicyScope` to `MessageBody` in the subscription\. 