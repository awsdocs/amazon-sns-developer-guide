# Amazon SNS message attributes<a name="sns-message-attributes"></a>

Amazon SNS supports delivery of message attributes, which let you provide structured metadata items \(such as timestamps, geospatial data, signatures, and identifiers\) about the message\. For attribute mapping between Amazon SNS and Amazon SQS, each message can have up to 10 attributes\. When using raw mode or an endpoint other than Amazon SQS, a message can have more than 10 attributes\.

Message attributes are optional and separate from—but are sent together with—the message body\. The receiver can use this information to decide how to handle the message without having to process the message body first\.

For information about sending messages with attributes using the AWS Management Console or the AWS SDK for Java, see the [Tutorial: Publishing a message with attributes to an Amazon SNS topic](sns-tutorial-publish-message-with-attributes.md) tutorial\.

**Note**  
Message attributes are sent only when the message structure is String, not JSON\.

You can also use message attributes to help structure the push notification message for mobile endpoints\. In this scenario, the message attributes are used only to help structure the push notification message\. The attributes are not delivered to the endpoint as they are when sending messages with message attributes to Amazon SQS endpoints\.

You can also use message attributes to make your messages filterable using subscription filter policies\. You can apply filter policies to topic subscriptions\. When a filter policy is applied, a subscription receives only those messages that have attributes that the policy accepts\. For more information, see [Amazon SNS message filtering](sns-message-filtering.md)\.

## Message attribute items and validation<a name="SNSMessageAttributesNTV"></a>

Each message attribute consists of the following items:
+ **Name** – The message attribute name can contain the following characters: A\-Z, a\-z, 0\-9, underscore\(\_\), hyphen\(\-\), and period \(\.\)\. The name must not start or end with a period, and it should not have successive periods\. The name is case\-sensitive and must be unique among all attribute names for the message\. The name can be up to 256 characters long\. The name cannot start with "AWS\." or "Amazon\." \(or any variations in casing\) because these prefixes are reserved for use by Amazon Web Services\.
+ **Type** – The supported message attribute data types are `String`, `String.Array`, `Number`, and `Binary`\. The data type has the same restrictions on the content as the message body\. The data type is case\-sensitive, and it can be up to 256 bytes long\. For more information, see the [Message attribute data types and validation](#SNSMessageAttributes.DataTypes) section\.
+ **Value** – The user\-specified message attribute value\. For string data types, the value attribute has the same restrictions on the content as the message body\. For more information, see the [Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html) action in the *Amazon Simple Notification Service API Reference*\.

Name, type, and value must not be empty or null\. In addition, the message body should not be empty or null\. All parts of the message attribute, including name, type, and value, are included in the message size restriction, which is 256 KB\.

## Message attribute data types and validation<a name="SNSMessageAttributes.DataTypes"></a>

Message attribute data types identify how the message attribute values are handled by Amazon SNS\. For example, if the type is a number, Amazon SNS validates that it's a number\.

Amazon SNS supports the following logical data types:
+ **String** – Strings are Unicode with UTF\-8 binary encoding\. For a list of code values, see [http://en\.wikipedia\.org/wiki/ASCII\#ASCII\_printable\_characters](http://en.wikipedia.org/wiki/ASCII#ASCII_printable_characters)\.
+ **String\.Array** – An array, formatted as a string, that can contain multiple values\. The values can be strings, numbers, or the keywords `true`, `false`, and `null`\.
+ **Number** – Numbers are positive or negative integers or floating\-point numbers\. Numbers have sufficient range and precision to encompass most of the possible values that integers, floats, and doubles typically support\. A number can have a value from \-109 to 109, with 5 digits of accuracy after the decimal point\. Leading and trailing zeroes are trimmed\.
+ **Binary** – Binary type attributes can store any binary data; for example, compressed data, encrypted data, or images\.

## Reserved message attributes for mobile push notifications<a name="sns-attrib-mobile-reserved"></a>

The following table lists the reserved message attributes for mobile push notification services that you can use to structure your push notification message: 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-message-attributes.html)