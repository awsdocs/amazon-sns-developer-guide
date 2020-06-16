# Tutorial: Publishing a message with attributes to an Amazon SNS topic<a name="sns-tutorial-publish-message-with-attributes"></a>

After you [create a topic](sns-tutorial-create-topic.md) and [subscribe an endpoint to it](sns-tutorial-create-subscribe-endpoint-to-topic.md), you can *publish* messages to a topic with message attributes, which let you provide structure metadata items about the message\. For more information, see [Amazon SNS message attributes](sns-message-attributes.md)\.

The following tutorial shows how you can use the AWS Management Console, the AWS SDK for Java, and the AWS SDK for \.NET to publish a message to a topic\.

**Topics**
+ [AWS Management Console](#publish-message-with-attributes-aws-console)
+ [AWS SDK for Java](#publish-message-with-attributes-aws-java)
+ [AWS SDK for \.NET](#publish-message-with-attributes-aws-dot-net)

## To publish a message with attributes to an Amazon SNS topic using the AWS Management Console<a name="publish-message-with-attributes-aws-console"></a>

For detailed instructions on publishing a message with attributes to an Amazon SNS topic using the AWS Management Console, see [To publish a message to an Amazon SNS topic using the AWS Management Console](sns-tutorial-publish-message-to-topic.md#publish-message-to-topic-aws-console)\.

## To publish a message with attributes to an Amazon SNS topic using the AWS SDK for Java<a name="publish-message-with-attributes-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

   The following code example helps simplify the process of publishing messages with attributes\. The `SNSMessageAttributes` class stores the `messageAttributes` field as a map\. You can use the overloaded `addAttribute` method to add attributes with the data types `String`, `String.Array`, and `Number`\. To publish the message, use the `publish` method by providing the `AmazonSNS` client and the topic ARN\.

   ```
   import com.amazonaws.services.sns.AmazonSNS;
   import com.amazonaws.services.sns.model.MessageAttributeValue;
   import com.amazonaws.services.sns.model.PublishRequest;
   import com.amazonaws.services.sns.model.PublishResult;
   
   import java.util.ArrayList;
   import java.util.HashMap;
   import java.util.Map;
   import java.util.stream.Collectors;
   
   public class SNSMessageAttributes {
   
       private String message;
       private Map<String, MessageAttributeValue> messageAttributes;
   
       public SNSMessageAttributes(final String message) {
           this.message = message;
           messageAttributes = new HashMap<>();
       }
   
       public String getMessage() {
           return message;
       }
   
       public void setMessage(final String message) {
           this.message = message;
       }
   
       public void addAttribute(final String attributeName, final String attributeValue) {
           final MessageAttributeValue messageAttributeValue = new MessageAttributeValue()
                   .withDataType("String")
                   .withStringValue(attributeValue);
           messageAttributes.put(attributeName, messageAttributeValue);
       }
   
       public void addAttribute(final String attributeName, final ArrayList<?> attributeValues) {
           String valuesString, delimiter = ", ", prefix = "[", suffix = "]";
           if (attributeValues.get(0).getClass() == String.class) {
               delimiter = "\", \"";
               prefix = "[\"";
               suffix = "\"]";
           }
           valuesString = attributeValues
                   .stream()
                   .map(Object::toString)
                   .collect(Collectors.joining(delimiter, prefix, suffix));
           final MessageAttributeValue messageAttributeValue = new MessageAttributeValue()
                   .withDataType("String.Array")
                   .withStringValue(valuesString);
           messageAttributes.put(attributeName, messageAttributeValue);
       }
   
       public void addAttribute(final String attributeName, final Number attributeValue) {
           final MessageAttributeValue messageAttributeValue = new MessageAttributeValue()
                   .withDataType("Number")
                   .withStringValue(attributeValue.toString());
           messageAttributes.put(attributeName, messageAttributeValue);
       }
   
       public String publish(final AmazonSNS snsClient, final String topicArn) {
           final PublishRequest request = new PublishRequest(topicArn, message)
                   .withMessageAttributes(messageAttributes);
           final PublishResult result = snsClient.publish(request);
           return result.getMessageId();
       }
   }
   ```

   The following code excerpt initializes and uses the example `SNSMessageAttributes` class\.

   ```
   // Initialize the example class.
   final SNSMessageAttributes message = new SNSMessageAttributes(messageBody);
   
   // Add message attributes with string values.
   message.addAttribute("store", "example_corp");
   message.addAttribute("event", "order_placed");
   
   // Add a message attribute with a list of string values.
   final ArrayList<String> interestsValues = new ArrayList<String>();
   interestsValues.add("soccer");
   interestsValues.add("rugby");
   interestsValues.add("hockey");
   message.addAttribute("customer_interests", interestsValues);
   
   // Add a message attribute with a numeric value.
   message.addAttribute("price_usd", 1000);
   
   // Add a Boolean attribute for filtering using subscription filter policies.
   // The class applies the String.Array data type to this attribute, allowing it
   // to be evaluated by a filter policy.
   final ArrayList<Boolean> encryptedVal = new ArrayList<Boolean>();
   encryptedVal.add(false);
   message.addAttribute("encrypted", encryptedVal);
   
   // Publish the message.
   message.publish(snsClient, topicArn);
   
   // Print the MessageId of the message.
   System.out.println("MessageId: " + publishResponse.getMessageId());
   ```

1. Compile and run your code\.

   The message is published and the `MessageId` is printed, for example:

   ```
   MessageId: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```

## To publish a message with attributes to an Amazon SNS topic using the AWS SDK for \.NET<a name="publish-message-with-attributes-aws-dot-net"></a>

1. Specify your AWS credentials\. For more information, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) in the *AWS SDK for \.NET Developer Guide*\.

1. Write your code\. For more information, see [Programming with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-programming-techniques.html)\.

1. The following code example helps simplify the process of publishing messages with attributes\. The `SNSMessageAttributes` class stores the `MessageAttributes` field as a dictionary\. You can use the overloaded `addAttribute` method to add attributes with the data types `String`, `String.Array`, and `Number`\. To publish the message, use the `publish` method by providing the `AmazonSimpleNotificationServiceClient` client and the topic ARN\.

   ```
   using System;
   using System.Collections.Generic;
   using Amazon.SimpleNotificationService;
   using Amazon.SimpleNotificationService.Model;
   
   namespace SNSCreatePlatformEndpoint
   {
   
       class SNSMessageAttributes
       {
   
           private String message;
           private Dictionary<String, MessageAttributeValue> messageAttributes;
   
           public SNSMessageAttributes(String message)
           {
               this.message = message;
               messageAttributes = new Dictionary<string, MessageAttributeValue>();
           }
   
           public string Message
           {
               get => this.message;
               set => this.message = value;
           }
   
           public void AddAttribute(String attributeName, String attributeValue)
           {
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "String",
                   StringValue = attributeValue
               };
           }
   
           public void AddAttribute(String attributeName, float attributeValue)
           {
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "Number",
                   StringValue = attributeValue.ToString()
               };
           }
   
           public void AddAttribute(String attributeName, int attributeValue)
           {
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "Number",
                   StringValue = attributeValue.ToString()
               };
           }
   
           public void AddAttribute(String attributeName, List<String> attributeValue)
           {
               String valueString = "[\"" + String.Join("\", \"", attributeValue.ToArray()) + "\"]";
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "String.Array",
                   StringValue = valueString
               };
           }
   
           public void AddAttribute(String attributeName, List<float> attributeValue)
           {
               String valueString = "[" + String.Join(", ", attributeValue.ToArray()) + "]";
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "String.Array",
                   StringValue = valueString
               };
           }
   
           public void AddAttribute(String attributeName, List<int> attributeValue)
           {
               String valueString = "[" + String.Join(", ", attributeValue.ToArray()) + "]";
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "String.Array",
                   StringValue = valueString
               };
           }
   
           public void AddAttribute(String attributeName, List<Boolean> attributeValue)
           {
               String valueString = "[" + String.Join(", ", attributeValue.ToArray()) + "]";
               messageAttributes[attributeName] = new MessageAttributeValue
               {
                   DataType = "String.Array",
                   StringValue = valueString
               };
           }
   
           public String Publish(AmazonSimpleNotificationServiceClient snsClient, String topicArn)
           {
               PublishRequest request = new PublishRequest
               {
                   TopicArn = topicArn,
                   MessageAttributes = messageAttributes,
                   Message = message
               };
               PublishResponse result = snsClient.Publish(request);
               return result.MessageId;
           }
       }
   }
   ```

   The following code excerpt initializes and uses the example `SNSMessageAttributes` class\.

   ```
   // Initialize the example class.
   SNSMessageAttributes message = new SNSMessageAttributes(messageBody);
   
   // Add message attributes with string values.
   message.AddAttribute("store", "example_corp");
   message.AddAttribute("event", "order_placed");
   
   // Add a message attribute with a list of string values.
   List<String> interestsValues = new List<String>();
   interestsValues.Add("soccer");
   interestsValues.Add("rugby");
   interestsValues.Add("hockey");
   message.AddAttribute("customer_interests", interestsValues);
   
   // Add a message attribute with a numeric value.
   message.AddAttribute("price_usd", 1000);
   
   // Add a Boolean attribute for filtering using subscription filter policies.
   // The class applies a String.Array data type to this attribute, allowing it
   // to be evaluated by a filter policy.
   List<Boolean> encryptedVal = new List<Boolean>();
   encryptedVal.Add(false);
   message.AddAttribute("encrypted", encryptedVal);
   
   // Publish the message.
   String msgId = message.Publish(snsClient, topicArn);
   
   // Print the MessageId of the published message.
   Console.WriteLine("MessageId: " + msgId);
   ```

1. Compile and run your code\.

   The message is published and the `MessageId` is printed, for example:

   ```
   MessageId: 1234a567-bc89-012d-3e45-6fg7h890123i
   ```