# Application event notifications<a name="application-event-notifications"></a>

Amazon SNS provides support to trigger notifications when certain application events occur\. You can then take some programmatic action on that event\. Your application must include support for a push notification service such as Apple Push Notification Service \(APNs\), Firebase Cloud Messaging \(FCM\), and Windows Push Notification Services \(WNS\)\. You set application event notifications using the Amazon SNS console, AWS CLI, or the AWS SDKs\.

**Topics**
+ [Available application events](#application-event-notifications-events)
+ [Sending mobile push notifications](#application-event-notifications-howto-set)

## Available application events<a name="application-event-notifications-events"></a>

Application event notifications track when individual platform endpoints are created, deleted, and updated, as well as delivery failures\. The following are the attribute names for the application events\.


| Attribute name | Notification trigger | 
| --- | --- | 
| EventEndpointCreated | A new platform endpoint is added to your application\. | 
| EventEndpointDeleted | Any platform endpoint associated with your application is deleted\. | 
| EventEndpointUpdated | Any of the attributes of the platform endpoints associated with your application are changed\. | 
| EventDeliveryFailure | A delivery to any of the platform endpoints associated with your application encounters a permanent failure\.  To track delivery failures on the platform application side, subscribe to message delivery status events for the application\. For more information, see [Using Amazon SNS Application Attributes for Message Delivery Status](https://docs.aws.amazon.com/sns/latest/dg/sns-msg-status.html)\.  | 

You can associate any attribute with an application which can then receive these event notifications\. 

## Sending mobile push notifications<a name="application-event-notifications-howto-set"></a>

To send application event notifications, you specify a topic to receive the notifications for each type of event\. As Amazon SNS sends the notifications, the topic can route them to endpoints that will take programmatic action\.

**Important**  
High\-volume applications will create a large number of application event notifications \(for example, tens of thousands\), which will overwhelm endpoints meant for human use, such as email addresses, phone numbers, and mobile applications\. Consider the following guidelines when you send application event notifications to a topic:  
Each topic that receives notifications should contain only subscriptions for programmatic endpoints, such as HTTP or HTTPS endpoints, Amazon SQS queues, or AWS Lambda functions\.
To reduce the amount of processing that is triggered by the notifications, limit each topic's subscriptions to a small number \(for example, five or fewer\)\.

You can send application event notifications using the Amazon SNS console, the AWS Command Line Interface \(AWS CLI\), or the AWS SDKs\. 

### AWS Management Console<a name="application-event-notifications-howto-set-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Mobile**, **Push notifications**\.

1. On the **Mobile push notifications** page, in the the **Platform applications** section, choose an application and then choose **Edit**\.

1. Expand the **Event notifications** section\.

1. Choose **Actions**, **Configure events**\.

1. Enter the ARNs for topics to be used for the following events:
   + Endpoint Created
   + Endpoint Deleted
   + Endpoint Updated
   + Delivery Failure

1. Choose **Save changes**\.

### AWS CLI<a name="awscli"></a>

Run the [set\-platform\-application\-attributes](https://docs.aws.amazon.com/cli/latest/reference/sns/set-platform-application-attributes.html) command\.

The following example sets the same Amazon SNS topic for all four application events:

```
aws sns set-platform-application-attributes
--platform-application-arn arn:aws:sns:us-east-1:12345EXAMPLE:app/FCM/MyFCMPlatformApplication
--attributes EventEndpointCreated="arn:aws:sns:us-east-1:12345EXAMPLE:MyFCMPlatformApplicationEvents",
EventEndpointDeleted="arn:aws:sns:us-east-1:12345EXAMPLE:MyFCMPlatformApplicationEvents",
EventEndpointUpdated="arn:aws:sns:us-east-1:12345EXAMPLE:MyFCMPlatformApplicationEvents",
EventDeliveryFailure="arn:aws:sns:us-east-1:12345EXAMPLE:MyFCMPlatformApplicationEvents"
```

### AWS SDKs<a name="application-event-notifications-sdk"></a>

Call one of the following APIs, depending on your target programming language or platform:


| Programming language or platform | API reference links | 
| --- | --- | 
| Android | [setPlatformApplicationAttributes](https://docs.aws.amazon.com/AWSAndroidSDK/latest/javadoc/com/amazonaws/services/sns/AmazonSNSClient.html#setPlatformApplicationAttributes%28com.amazonaws.services.sns.model.SetPlatformApplicationAttributesRequest%29) | 
| iOS | [AWSSNSSetPlatformApplicationAttributesInput](https://aws-amplify.github.io/aws-sdk-ios/docs/reference/AWSSNS/Classes/AWSSNSSetPlatformApplicationAttributesInput.html) | 
| Java | [setPlatformApplicationAttributes](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sns/AmazonSNSClient.html#setPlatformApplicationAttributes(com.amazonaws.services.sns.model.SetPlatformApplicationAttributesRequest)) | 
| JavaScript | [setPlatformApplicationAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setPlatformApplicationAttributes-property) | 
| \.NET | [SetPlatformApplicationAttributes](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SNS/MSNSSNSSetPlatformApplicationAttributesSetPlatformApplicationAttributesRequest.html&tocid=Amazon_SimpleNotificationService_AmazonSimpleNotificationServiceClient) | 
| PHP | [SetPlatformApplicationAttributes](https://docs.aws.amazon.com/aws-sdk-php/v3/api/api-sns-2010-03-31.html#setplatformapplicationattributes) | 
| Python \(boto\) | [set\_platform\_application\_attributes](http://boto.readthedocs.org/en/latest/ref/sns.html) | 
| Ruby | [set\_platform\_application\_attributes](https://docs.aws.amazon.com/sdkforruby/api/Aws/SNS/Client.html#set_platform_application_attributes-instance_method) | 
| Unity | [SetPlatformApplicationAttributesAsync](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=SNS/MSNSSNSSetPlatformApplicationAttributesAsyncSetPlatformApplicationAttributesRequestCancellationToken.html&tocid=Amazon_SimpleNotificationService_AmazonSimpleNotificationServiceClient) | 
| Windows PowerShell | [Set\-SNSPlatformApplicationAttributes](https://docs.aws.amazon.com/powershell/latest/reference/items/Set-SNSPlatformApplicationAttributes.html) | 