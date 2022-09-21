# Verifying the signatures of Amazon SNS messages<a name="sns-verify-signature-of-message"></a>

To verify the authenticity of a message sent to your HTTP endpoint by Amazon SNS, you can verify the message signature\. There are two cases where we recommend verifying the authenticity of the message\. First, when Amazon SNS sends a message to your HTTP endpoint that you subscribed to a topic\. Second, when Amazon SNS sends you a confirmation message to your HTTP endpoint upon the execution of the `Subscribe` or the `Unsubscribe` API actions\.

You should do the following when verifying messages sent by Amazon SNS:
+ Always use HTTPS when getting the certificate from Amazon SNS\.
+ Validate the authenticity of the certificate\.
+ Verify the certificate was received from Amazon SNS\.
+ When possible, use one of the supported AWS SDKs for Amazon SNS to validate and verify messages\.

Amazon SNS supports two message signature versions: 
+ `SignatureVersion`**1**: Amazon SNS creates the signature based on the **SHA1** hash of the message\. 
+ `SignatureVersion`**2**: Amazon SNS creates the signature based on the **SHA256** hash of the message\. 

**To configure the message signature version on Amazon SNS topics**

By default, Amazon SNS topics use `SignatureVersion` 1\. To choose the hashing algorithm on your Amazon SNS topic, either `SignatureVersion` **1** \(SHA1\) or `SignatureVersion` 2** **\(SHA256\), you can use the `SetTopicAttributes` API action\.

The following code example shows how to set the topic attribute `SignatureVersion` using the AWS CLI:

```
aws sns set-topic-attributes \
    --topic-arn arn:aws:sns:us-east-2:123456789012:MyTopic \
    --attribute-name SignatureVersion \
    --attribute-value 2
```

**To verify the signature of an Amazon SNS message when using HTTP query\-based requests**

1. Extract the name\-value pairs from the JSON document in the body of the HTTP POST request that Amazon SNS sent to your endpoint\. You'll be using the values of some of the name\-value pairs to create the string to sign\. When you are verifying the signature of an Amazon SNS message, it is critical that you convert the escaped control characters to their original character representations in the `Message` and `Subject` values\. These values must be in their original forms when you use them as part of the string to sign\. For information about how to parse the JSON document, see [Step 1: Make sure your endpoint is ready to process Amazon SNS messages](SendMessageToHttp.prepare.md)\. 

   The `SignatureVersion` tells you the signature version used by Amazon SNS to generate the signature of the message\. From the signature version, you can determine the requirements for how to generate the signature\. For notifications, Amazon SNS currently supports signature version **1** and **2**\. This section provides the steps for verifying a signature using these signature versions\.

1. Get the X509 certificate that Amazon SNS used to sign the message\. The `SigningCertURL` value points to the location of the X509 certificate used to create the digital signature for the message\. Retrieve the certificate from this location\. 

1. Extract the public key from the certificate\. The public key from the certificate specified by `SigningCertURL` is used to verify the authenticity and integrity of the message\. 

1. Determine the message type\. The format of the string to sign depends on the message type, which is specified by the `Type` value\.

1. Create the string to sign\. The string to sign is a newline character–delimited list of specific name\-value pairs from the message\. Each name\-value pair is represented with the name first followed by a newline character, followed by the value, and ending with a newline character\. The name\-value pairs must be listed in byte\-sort order\.

   Depending on the message type, the string to sign must have the following name\-value pairs\.  
**Notification**  
Notification messages must contain the following name\-value pairs:  

   ```
   Message
   MessageId
   Subject (if included in the message)
   Timestamp
   TopicArn
   Type
   ```
The following example is a string to sign for a `Notification`\.  

   ```
   Message
   My Test Message
   MessageId
   4d4dc071-ddbf-465d-bba8-08f81c89da64
   Subject
   My subject
   Timestamp
   2019-01-31T04:37:04.321Z
   TopicArn
   arn:aws:sns:us-east-2:123456789012:s4-MySNSTopic-1G1WEFCOXTC0P
   Type
   Notification
   ```  
**SubscriptionConfirmation and UnsubscribeConfirmation**  
`SubscriptionConfirmation` and `UnsubscribeConfirmation` messages must contain the following name\-value pairs:  

   ```
   Message
   MessageId
   SubscribeURL
   Timestamp
   Token
   TopicArn
   Type
   ```
The following example is a string to sign for a `SubscriptionConfirmation`\.  

   ```
   Message
   My Test Message
   MessageId
   3d891288-136d-417f-bc05-901c108273ee
   SubscribeURL
   https://sns.us-east-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-east-2:123456789012:s4-MySNSTopic-1G1WEFCOXTC0P&Token=233...
   Timestamp
   2019-01-31T19:25:13.719Z
   Token
   233...
   TopicArn
   arn:aws:sns:us-east-2:123456789012:s4-MySNSTopic-1G1WEFCOXTC0P
   Type
   SubscriptionConfirmation
   ```

1. Decode the `Signature` value from Base64 format\. The message delivers the signature in the `Signature` value, which is encoded as Base64\. Before you compare the signature value with the signature you have calculated, make sure that you decode the `Signature` value from Base64 so that you compare the values using the same format\.

1. Generate the derived hash value of the Amazon SNS message\. Submit the Amazon SNS message, in canonical format, to the same hash algorithm used to generate the signature\.

   1. If the `SignatureVersion` is **1**, use **SHA1** as the hash algorithm\.

   1. If the `SignatureVersion` is **2**, use **SHA256** as the hash algorithm\.

1. Generate the asserted hash value of the Amazon SNS message\. The asserted hash value is the result of using the public key value \(from step 3\) to decrypt the signature delivered with the Amazon SNS message\. 

1. Verify the authenticity and integrity of the Amazon SNS message\. Compare the derived hash value \(from step 7\) to the asserted hash value \(from step 8\)\. If the values are identical, then the receiver is assured that the message has not been modified while in transit and the message must have originated from Amazon SNS\. If the values are not identical, it should not be trusted by the receiver\. 