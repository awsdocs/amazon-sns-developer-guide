# Step 1: Make sure your endpoint is ready to process Amazon SNS messages<a name="SendMessageToHttp.prepare"></a>

Before you subscribe your HTTP or HTTPS endpoint to a topic, you must make sure that the HTTP or HTTPS endpoint has the capability to handle the HTTP POST requests that Amazon SNS uses to send the subscription confirmation and notification messages\. Usually, this means creating and deploying a web application \(for example, a Java servlet if your endpoint host is running Linux with Apache and Tomcat\) that processes the HTTP requests from Amazon SNS\. When you subscribe an HTTP endpoint, Amazon SNS sends it a subscription confirmation request\. Your endpoint must be prepared to receive and process this request when you create the subscription because Amazon SNS sends this request at that time\. Amazon SNS will not send notifications to the endpoint until you confirm the subscription\. Once you confirm the subscription, Amazon SNS will send notifications to the endpoint when a publish action is performed on the subscribed topic\.

**To set up your endpoint to process subscription confirmation and notification messages**

1. Your code should read the HTTP headers of the HTTP POST requests that Amazon SNS sends to your endpoint\. Your code should look for the header field `x-amz-sns-message-type`, which tells you the type of message that Amazon SNS has sent to you\. By looking at the header, you can determine the message type without having to parse the body of the HTTP request\. There are two types that you need to handle: `SubscriptionConfirmation` and `Notification`\. The `UnsubscribeConfirmation` message is used only when the subscription is deleted from the topic\.

   For details about the HTTP header, see [HTTP/HTTPS headers](sns-message-and-json-formats.md#http-header)\. The following HTTP POST request is an example of a subscription confirmation message\.

   ```
   POST / HTTP/1.1
       x-amz-sns-message-type: SubscriptionConfirmation
       x-amz-sns-message-id: 165545c9-2a5c-472c-8df2-7ff2be2b3b1b
       x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
       Content-Length: 1336
       Content-Type: text/plain; charset=UTF-8
       Host: example.com
       Connection: Keep-Alive
       User-Agent: Amazon Simple Notification Service Agent
       
   {
     "Type" : "SubscriptionConfirmation",
     "MessageId" : "165545c9-2a5c-472c-8df2-7ff2be2b3b1b",
     "Token" : "2336412f37f...",
     "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
     "Message" : "You have chosen to subscribe to the topic arn:aws:sns:us-west-2:123456789012:MyTopic.\nTo confirm the subscription, visit the SubscribeURL included in this message.",
     "SubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37...",
     "Timestamp" : "2012-04-26T20:45:04.751Z",
     "SignatureVersion" : "1",
     "Signature" : "EXAMPLEpH+...",
     "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem"
   }
   ```

1. Your code should parse the JSON document in the body of the HTTP POST request to read the name\-value pairs that make up the Amazon SNS message\. Use a JSON parser that handles converting the escaped representation of control characters back to their ASCII character values \(for example, converting \\n to a newline character\)\. You can use an existing JSON parser such as the [Jackson JSON Processor](https://github.com/FasterXML/jackson) or write your own\. In order to send the text in the subject and message fields as valid JSON, Amazon SNS must convert some control characters to escaped representations that can be included in the JSON document\. When you receive the JSON document in the body of the POST request sent to your endpoint, you must convert the escaped characters back to their original character values if you want an exact representation of the original subject and messages published to the topic\. This is critical if you want to verify the signature of a notification because the signature uses the message and subject in their original forms as part of the string to sign\.

1. Your code should verify the authenticity of a notification, subscription confirmation, or unsubscribe confirmation message sent by Amazon SNS\. Using information contained in the Amazon SNS message, your endpoint can recreate the signature so that you can verify the contents of the message by matching your signature with the signature that Amazon SNS sent with the message\. For more information about verifying the signature of a message, see [Verifying the signatures of Amazon SNS messages](sns-verify-signature-of-message.md)\.

1. Based on the type specified by the header field `x-amz-sns-message-type`, your code should read the JSON document contained in the body of the HTTP request and process the message\. Here are the guidelines for handling the two primary types of messages:  
**SubscriptionConfirmation**  
Read the value for `SubscribeURL` and visit that URL\. To confirm the subscription and start receiving notifications at the endpoint, you must visit the `SubscribeURL`URL \(for example, by sending an HTTP GET request to the URL\)\. See the example HTTP request in the previous step to see what the `SubscribeURL` looks like\. For more information about the format of the `SubscriptionConfirmation` message, see [HTTP/HTTPS subscription confirmation JSON format](sns-message-and-json-formats.md#http-subscription-confirmation-json)\. When you visit the URL, you will get back a response that looks like the following XML document\. The document returns the subscription ARN for the endpoint within the `ConfirmSubscriptionResult` element\.  

   ```
   <ConfirmSubscriptionResponse xmlns="http://sns.amazonaws.com/doc/2010-03-31/">
      <ConfirmSubscriptionResult>
         <SubscriptionArn>arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55</SubscriptionArn>
      </ConfirmSubscriptionResult>
      <ResponseMetadata>
         <RequestId>075ecce8-8dac-11e1-bf80-f781d96e9307</RequestId>
      </ResponseMetadata>
   </ConfirmSubscriptionResponse>
   ```
As an alternative to visiting the `SubscribeURL`, you can confirm the subscription using the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) action with the `Token` set to its corresponding value in the `SubscriptionConfirmation` message\. If you want to allow only the topic owner and subscription owner to be able to unsubscribe the endpoint, you call the `ConfirmSubscription` action with an AWS signature\.  
**Notification**  
Read the values for `Subject` and `Message` to get the notification information that was published to the topic\.  
For details about the format of the `Notification` message, see [HTTP/HTTPS headers](sns-message-and-json-formats.md#http-header)\. The following HTTP POST request is an example of a notification message sent to the endpoint example\.com\.  

   ```
   POST / HTTP/1.1
       x-amz-sns-message-type: Notification
       x-amz-sns-message-id: 22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324
       x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
       x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96
       Content-Length: 773
       Content-Type: text/plain; charset=UTF-8
       Host: example.com
       Connection: Keep-Alive
       User-Agent: Amazon Simple Notification Service Agent
       
   {
     "Type" : "Notification",
     "MessageId" : "22b80b92-fdea-4c2c-8f9d-bdfb0c7bf324",
     "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
     "Subject" : "My First Message",
     "Message" : "Hello world!",
     "Timestamp" : "2012-05-02T00:54:06.655Z",
     "SignatureVersion" : "1",
     "Signature" : "EXAMPLEw6JRN...",
     "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
     "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:c9135db0-26c4-47ec-8998-413945fb5a96"
   }
   ```

1. Make sure that your endpoint responds to the HTTP POST message from Amazon SNS with the appropriate status code\. The connection will time out in 15 seconds\. If your endpoint does not respond before the connection times out or if your endpoint returns a status code outside the range of 200–4*xx*, Amazon SNS will consider the delivery of the message as a failed attempt\.

1. Make sure that your code can handle message delivery retries from Amazon SNS\. If Amazon SNS doesn't receive a successful response from your endpoint, it attempts to deliver the message again\. This applies to all messages, including the subscription confirmation message\. By default, if the initial delivery of the message fails, Amazon SNS attempts up to three retries with a delay between failed attempts set at 20 seconds\.
**Note**  
The message request times out after 15 seconds\. This means that, if the message delivery failure is caused by a timeout, Amazon SNS retries for approximately 35 seconds after the previous delivery attempt\. You can set a different delivery policy for the endpoint\.

   To be clear, Amazon SNS attempts to retry only after a delivery `x-amz-sns-message-id` header field\. By comparing the IDs of the messages you have processed with incoming messages, you can determine whether the message is a retry attempt\.

1. If you are subscribing an HTTPS endpoint, make sure that your endpoint has a server certificate from a trusted Certificate Authority \(CA\)\. Amazon SNS will only send messages to HTTPS endpoints that have a server certificate signed by a CA trusted by Amazon SNS\.

1. Deploy the code that you have created to receive Amazon SNS messages\. When you subscribe the endpoint, the endpoint must be ready to receive at least the subscription confirmation message\.