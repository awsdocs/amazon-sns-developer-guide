# Amazon SNS message delivery status<a name="sns-topic-attributes"></a>

Amazon SNS provides support to log the delivery status of notification messages sent to topics with the following Amazon SNS endpoints: 
+ Application
+ HTTP
+ Lambda
+ SQS

 After you configure the message delivery status attributes, log entries will be sent to CloudWatch Logs for messages sent to a topic subscribed to an Amazon SNS endpoint\. Logging message delivery status helps provide better operational insight, such as the following: 
+ Knowing whether a message was delivered to the Amazon SNS endpoint\.
+ Identifying the response sent from the Amazon SNS endpoint to Amazon SNS\.
+ Determining the message dwell time \(the time between the publish timestamp and just before handing off to an Amazon SNS endpoint\)\.

 To configure topic attributes for message delivery status, you can use the AWS Management Console, AWS software development kits \(SDKs\), or query API\. 

**Topics**
+ [Configuring delivery status logging using the AWS Management Console](#topics-attrib)
+ [Configuring message delivery status attributes for topics subscribed to Amazon SNS endpoints using the AWS SDKs](#msg-status-sdk)

## Configuring delivery status logging using the AWS Management Console<a name="topics-attrib"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and then choose **Edit**\.

1. On the **Edit *MyTopic*** page, expand the **Delivery status logging** section\.

1. Choose the protocols for which you want to log delivery status, for example **Lambda**\.

1. Enter the **Success sample rate** \(the percentage of successful messages for which you want to receive CloudWatch Logs\.

1. In the **IAM roles** subsection, do one of the following:
   + To choose an existing service role from your account, choose **Use existing service role** and then specify IAM roles for successful and failed deliveries\.
   + To create a new service role in your account, choose **Create new service role**, choose **Create new roles** to define the IAM roles for successful and failed deliveries in the IAM console\.

     To give Amazon SNS write access to use CloudWatch Logs on your behalf, choose **Allow**\.

1. Choose **Save changes**\.

   You can now view and parse the CloudWatch Logs containing the message delivery status\. For more information about using CloudWatch, see the [CloudWatch Documentation](https://aws.amazon.com/documentation/cloudwatch)\.

## Configuring message delivery status attributes for topics subscribed to Amazon SNS endpoints using the AWS SDKs<a name="msg-status-sdk"></a>

The [AWS SDKs](https://aws.amazon.com/tools/) provide APIs in several languages for using message delivery status attributes with Amazon SNS\. 

### Topic attributes<a name="topic-attributes"></a>

You can use the following topic attribute name values for message delivery status:

**Application**
+ `ApplicationSuccessFeedbackRoleArn`
+ `ApplicationSuccessFeedbackSampleRate`
+ `ApplicationFailureFeedbackRoleArn`
**Note**  
In addition to being able to configure topic attributes for message delivery status of notification messages sent to Amazon SNS application endpoints, you can also configure application attributes for the delivery status of push notification messages sent to push notification services\. For more information, see [Using Amazon SNS Application Attributes for Message Delivery Status](https://docs.aws.amazon.com/sns/latest/dg/sns-msg-status.html)\. 

**HTTP**
+ `HTTPSuccessFeedbackRoleArn`
+ `HTTPSuccessFeedbackSampleRate`
+ `HTTPFailureFeedbackRoleArn`

**Lambda**
+ `LambdaSuccessFeedbackRoleArn`
+ `LambdaSuccessFeedbackSampleRate`
+ `LambdaFailureFeedbackRoleArn`

**SQS**
+ `SQSSuccessFeedbackRoleArn`
+ `SQSSuccessFeedbackSampleRate`
+ `SQSFailureFeedbackRoleArn`

 The `<ENDPOINT>SuccessFeedbackRoleArn` and `<ENDPOINT>FailureFeedbackRoleArn` attributes are used to give Amazon SNS write access to use CloudWatch Logs on your behalf\. The `<ENDPOINT>SuccessFeedbackSampleRate` attribute is for specifying the sample rate percentage \(0\-100\) of successfully delivered messages\. After you configure the `<ENDPOINT>FailureFeedbackRoleArn` attribute, then all failed message deliveries generate CloudWatch Logs\. 

### AWS SDK examples to configure topic attributes<a name="topic-attributes-sdks"></a>

The following examples show how to configure topic attributes using the Amazon SNS clients that are provided by the AWS SDKs\.

------
#### [ AWS SDK for Java ]

The following Java example shows how to use the `SetTopicAttributes` API to configure topic attributes for message delivery status of notification messages sent to topics subscribed to Amazon SNS endpoints\. In this example, it is assumed that string values have been set for `topicArn`, `attribName`, and `attribValue`\.

```
final static String topicArn = ("arn:aws:sns:us-east-2:123456789012:MyTopic");
final static String attribName = ("LambdaSuccessFeedbackRoleArn");
final static String attribValue = ("arn:aws:iam::123456789012:role/SNSSuccessFeedback");
```

```
SetTopicAttributesRequest setTopicAttributesRequest = new SetTopicAttributesRequest();
setTopicAttributesRequest.withTopicArn(topicArn);
setTopicAttributesRequest.setAttributeName(attribName);
setTopicAttributesRequest.setAttributeValue(attribValue);
```

For more information about the SDK for Java, see [Getting Started with the AWS SDK for Java](https://aws.amazon.com/developers/getting-started/java/)\.

------
#### [ AWS SDK for \.NET ]

The following \.NET example shows how to use the `SetTopicAttributes` API to configure topic attributes for message delivery status of notification messages sent to topics subscribed to Amazon SNS endpoints\. In this example, it is assumed that string values have been set for `topicArn`, `attribName`, and `attribValue`\.

```
static String topicArn = "arn:aws:sns:us-east-2:123456789012:MyTopic";
static String attribName = "LambdaSuccessFeedbackRoleArn";
String attribValue = "arn:aws:iam::123456789012:role/SNSSuccessFeedback";
```

```
SetTopicAttributesRequest setTopicAttributesRequest = new SetTopicAttributesRequest {
    TopicArn = topicArn,
    AttributeName = attribName,
    AttributeValue = attribValue
};
```

For more information about the AWS SDK for \.NET, see [Getting Started with the AWS SDK for \.NET](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-setup.html)\.

------