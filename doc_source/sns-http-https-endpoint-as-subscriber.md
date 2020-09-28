# Fanout to HTTP/S endpoints<a name="sns-http-https-endpoint-as-subscriber"></a>

You can use [Amazon SNS](https://aws.amazon.com/sns/) to send notification messages to one or more HTTP or HTTPS endpoints\. When you subscribe an endpoint to a topic, you can publish a notification to the topic and Amazon SNS sends an HTTP POST request delivering the contents of the notification to the subscribed endpoint\. When you subscribe the endpoint, you choose whether Amazon SNS uses HTTP or HTTPS to send the POST request to the endpoint\. If you use HTTPS, then you can take advantage of the support in Amazon SNS for the following: 
+ **Server Name Indication \(SNI\)**—This allows Amazon SNS to support HTTPS endpoints that require SNI, such as a server requiring multiple certificates for hosting multiple domains\. For more information about SNI, see [Server Name Indication](http://en.wikipedia.org/wiki/Server_Name_Indication)\.
+ **Basic and Digest Access Authentication**—This allows you to specify a username and password in the HTTPS URL for the HTTP POST request, such as `https://user:password@domain.com` or `https://user@domain.com` The username and password are encrypted over the SSL connection established when using HTTPS\. Only the domain name is sent in plaintext\. For more information about Basic and Digest Access Authentication, see [RFC\-2617](http://www.rfc-editor.org/info/rfc2617)\.
**Note**  
 The client service must be able to support the `HTTP/1.1 401 Unauthorized` header response

The request contains the subject and message that were published to the topic along with metadata about the notification in a JSON document\. The request will look similar to the following HTTP POST request\. For details about the HTTP header and the JSON format of the request body, see [HTTP/HTTPS headers](sns-message-and-json-formats.md#http-header) and [HTTP/HTTPS notification JSON format](sns-message-and-json-formats.md#http-notification-json)\. 

```
POST / HTTP/1.1
    x-amz-sns-message-type: Notification
    x-amz-sns-message-id: da41e39f-ea4d-435a-b922-c6aae3915ebe
    x-amz-sns-topic-arn: arn:aws:sns:us-west-2:123456789012:MyTopic
    x-amz-sns-subscription-arn: arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55
    Content-Length: 761
    Content-Type: text/plain; charset=UTF-8
    Host: ec2-50-17-44-49.compute-1.amazonaws.com
    Connection: Keep-Alive
    User-Agent: Amazon Simple Notification Service Agent
    
{
  "Type" : "Notification",
  "MessageId" : "da41e39f-ea4d-435a-b922-c6aae3915ebe",
  "TopicArn" : "arn:aws:sns:us-west-2:123456789012:MyTopic",
  "Subject" : "test",
  "Message" : "test message",
  "Timestamp" : "2012-04-25T21:49:25.719Z",
  "SignatureVersion" : "1",
  "Signature" : "EXAMPLElDMXvB8r9R83tGoNn0ecwd5UjllzsvSvbItzfaMpN2nk5HVSw7XnOn/49IkxDKz8YrlH2qJXj2iZB0Zo2O71c4qQk1fMUDi3LGpij7RCW7AW9vYYsSqIKRnFS94ilu7NFhUzLiieYr4BKHpdTmdD6c0esKEYBpabxDSc=",
  "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem",
   "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-west-2:123456789012:MyTopic:2bcfbf39-05c3-41de-beaa-fcfcc21c8f55"
}
```

**Topics**
+ [Subscribing an endpoint to a topic](sns-subscribe-https-s-endpoints-to-topic.md)
+ [Verifying message signatures](sns-verify-signature-of-message.md)
+ [Parsing message formats](sns-message-and-json-formats.md)
+ [Example \(Java\)](sns-example-code-endpoint-java-servlet.md)