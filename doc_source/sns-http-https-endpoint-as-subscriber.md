# Using Amazon SNS for system\-to\-system messaging with an HTTP/s endpoint as a subscriber<a name="sns-http-https-endpoint-as-subscriber"></a>

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

To enable an Amazon SNS topic to send messages to an HTTP or HTTPS endpoint, follow these steps:

[Step 1: Make sure your endpoint is ready to process Amazon SNS messages](#SendMessageToHttp.prepare)

[Step 2: Subscribe the HTTP/HTTPS endpoint to the Amazon SNS topic](#SendMessageToHttp.subscribe)

[Step 3: Confirm the subscription](#SendMessageToHttp.confirm)

[Step 4: Set the delivery retry policy for the subscription \(optional\)](#SendMessageToHttp.retry)

[Step 5: Give users permissions to publish to the topic \(optional\)](#SendMessageToHttp.iam.permissions)

[Step 6: Send messages to the HTTP/HTTPS endpoint](#SendMessageToHttp.publish)

## Step 1: Make sure your endpoint is ready to process Amazon SNS messages<a name="SendMessageToHttp.prepare"></a>

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

## Step 2: Subscribe the HTTP/HTTPS endpoint to the Amazon SNS topic<a name="SendMessageToHttp.subscribe"></a>

To send messages to an HTTP or HTTPS endpoint through a topic, you must subscribe the endpoint to the Amazon SNS topic\. You specify the endpoint using its URL\. To subscribe to a topic, you can use the Amazon SNS console, the [sns\-subscribe](https://docs.aws.amazon.com/cli/latest/reference/sns/subscribe.html) command, or the [Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html) API action\. Before you start, make sure you have the URL for the endpoint that you want to subscribe and that your endpoint is prepared to receive the confirmation and notification messages as described in Step 1\.

**To subscribe an HTTP or HTTPS endpoint to a topic using the Amazon SNS console**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics** and then choose the topic\.

1. Choose the **Other actions** drop\-down list and select **Subscribe to topic**\.

1. In the **Protocol** drop\-down list, select **HTTP** or **HTTPS**\.

1. In the **Endpoint** box, paste in the URL for the endpoint that you want the topic to send messages to and then choose **Create subscription**\.

1. For the **Subscription request received\!** message, choose **Close**\.

   Your new subscription's **Subscription ID** displays PendingConfirmation\. When you confirm the subscription, **Subscription ID** will display the subscription ID\.

## Step 3: Confirm the subscription<a name="SendMessageToHttp.confirm"></a>

After you subscribe your endpoint, Amazon SNS will send a subscription confirmation message to the endpoint\. You should already have code that performs the actions described in [Step 1](#SendMessageToHttp.prepare) deployed to your endpoint\. Specifically, the code at the endpoint must retrieve the `SubscribeURL` value from the subscription confirmation message and either visit the location specified by `SubscribeURL` itself or make it available to you so that you can manually visit the `SubscribeURL`, for example, using a web browser\. Amazon SNS will not send messages to the endpoint until the subscription has been confirmed\. When you visit the `SubscribeURL`, the response will contain an XML document containing an element `SubscriptionArn` that specifies the ARN for the subscription\. You can also use the Amazon SNS console to verify that the subscription is confirmed: The **Subscription ID** will display the ARN for the subscription instead of the `PendingConfirmation` value that you saw when you first added the subscription\.

## Step 4: Set the delivery retry policy for the subscription \(optional\)<a name="SendMessageToHttp.retry"></a>

By default, if the initial delivery of the message fails, Amazon SNS attempts up to three retries with a delay between failed attempts set at 20 seconds\. As discussed in [Step 1](#SendMessageToHttp.prepare), your endpoint should have code that can handle retried messages\. By setting the delivery policy on a topic or subscription, you can control the frequency and interval that Amazon SNS will retry failed messages\. You can set a delivery policy on a topic or on a particular subscription\.

## Step 5: Give users permissions to publish to the topic \(optional\)<a name="SendMessageToHttp.iam.permissions"></a>

By default, the topic owner has permissions to publish the topic\. To enable other users or applications to publish to the topic, you should use AWS Identity and Access Management \(IAM\) to give publish permission to the topic\. For more information about giving permissions for Amazon SNS actions to IAM users, see [Using identity\-based policies with Amazon SNS](sns-using-identity-based-policies.md)\.

There are two ways to control access to a topic:
+ Add a policy to an IAM user or group\. The simplest way to give users permissions to topics is to create a group and add the appropriate policy to the group and then add users to that group\. It's much easier to add and remove users from a group than to keep track of which policies you set on individual users\.
+ Add a policy to the topic\. If you want to give permissions to a topic to another AWS account, the only way you can do that is by adding a policy that has as its principal the AWS account you want to give permissions to\.

You should use the first method for most cases \(apply policies to groups and manage permissions for users by adding or removing the appropriate users to the groups\)\. If you need to give permissions to a user in another account, use the second method\.

If you added the following policy to an IAM user or group, you would give that user or members of that group permission to perform the `sns:Publish` action on the topic MyTopic\.

```
{
  "Statement":[{
    "Sid":"AllowPublishToMyTopic",
    "Effect":"Allow",
    "Action":"sns:Publish",
    "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```

The following example policy shows how to give another account permissions to a topic\.

**Note**  
When you give another AWS account access to a resource in your account, you are also giving IAM users who have admin\-level access \(wildcard access\) permissions to that resource\. All other IAM users in the other account are automatically denied access to your resource\. If you want to give specific IAM users in that AWS account access to your resource, the account or an IAM user with admin\-level access must delegate permissions for the resource to those IAM users\. For more information about cross\-account delegation, see [Enabling Cross\-Account Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/Delegation.html) in the *Using IAM Guide*\.

If you added the following policy to a topic MyTopic in account 123456789012, you would give account 111122223333 permission to perform the `sns:Publish` action on that topic\.

```
{
  "Statement":[{
    "Sid":"Allow-publish-to-topic",
    "Effect":"Allow",
      "Principal":{
        "AWS":"111122223333"
      },
    "Action":"sns:Publish",
    "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```

## Step 6: Send messages to the HTTP/HTTPS endpoint<a name="SendMessageToHttp.publish"></a>

You can send a message to a topic's subscriptions by publishing to the topic\. To publish to a topic, you can use the Amazon SNS console, the `[sns\-publish](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html)` CLI command, or the `[Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html)` API\.

If you followed [Step 1](#SendMessageToHttp.prepare), the code that you deployed at your endpoint should process the notification\.

**To publish to a topic using the Amazon SNS console**

1. Using the credentials of the AWS account or IAM user with permission to publish to the topic, sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics** and then choose a topic\.

1. Choose the **Publish message** button\.

1. In the **Subject** box, enter a subject \(for example, **Testing publish to my endpoint**\)\.

1. In the **Message** box, enter some text \(for example, **Hello world\!**\), and choose **Publish message**\.

    The following message appears: Your message has been successfully published\.