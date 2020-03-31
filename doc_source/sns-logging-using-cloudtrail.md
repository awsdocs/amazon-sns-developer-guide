# Logging Amazon SNS API calls using CloudTrail<a name="sns-logging-using-cloudtrail"></a>

Amazon SNS is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon SNS\. CloudTrail captures API calls for Amazon SNS as events\. The calls captured include calls from the Amazon SNS console and code calls to the Amazon SNS API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon SNS\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon SNS, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Amazon SNS information in CloudTrail<a name="sns-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When supported event activity occurs in Amazon SNS, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon SNS, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

Amazon SNS supports logging the following actions as events in CloudTrail log files:
+ `[AddPermission](https://docs.aws.amazon.com/sns/latest/api/API_AddPermission.html)`
+ `[ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html)`
+ `[CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html)`
+ `[CreatePlatformEndpoint](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformEndpoint.html)`
+ `[CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html)`
+ `[DeleteEndpoint](https://docs.aws.amazon.com/sns/latest/api/API_DeleteEndpoint.html)`
+ `[DeletePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_DeletePlatformApplication.html)`
+ `[DeleteTopic](https://docs.aws.amazon.com/sns/latest/api/API_DeleteTopic.html)`
+ `[GetEndpointAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetEndpointAttributes.html)`
+ `[GetPlatformApplicationAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetPlatformApplicationAttributes.html)`
+ `[GetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetSMSAttributes.html)`
+ `[GetSubscriptionAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetSubscriptionAttributes.html)`
+ `[GetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_GetTopicAttributes.html)`
+ `[ListEndpointsByPlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_ListEndpointsByPlatformApplication.html)`
+ `[ListPhoneNumbersOptedOut](https://docs.aws.amazon.com/sns/latest/api/API_ListPhoneNumbersOptedOut.html)`
+ `[ListPlatformApplications](https://docs.aws.amazon.com/sns/latest/api/API_ListPlatformApplications.html)`
+ `[ListSubscriptions](https://docs.aws.amazon.com/sns/latest/api/API_ListSubscriptions.html)`
+ `[ListSubscriptionsByTopic](https://docs.aws.amazon.com/sns/latest/api/API_ListSubscriptionsByTopic.html)`
+ `[ListTopics](https://docs.aws.amazon.com/sns/latest/api/API_ListTopics.html)`
+ `[RemovePermission](https://docs.aws.amazon.com/sns/latest/api/API_RemovePermission.html)`
+ `[SetEndpointAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetEndpointAttributes.html)`
+ `[SetPlatformApplicationAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetPlatformApplicationAttributes.html)`
+ `[SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html)`
+ `[SetSubscriptionAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html)`
+ `[SetTopicAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetTopicAttributes.html)`
+ `[Subscribe](https://docs.aws.amazon.com/sns/latest/api/API_Subscribe.html)`
+ `[Unsubscribe](https://docs.aws.amazon.com/sns/latest/api/API_Unsubscribe.html)`

**Note**  
When you are not logged in to Amazon Web Services \(unauthenticated mode\) and either the [ConfirmSubscription](https://docs.aws.amazon.com/sns/latest/api/API_ConfirmSubscription.html) or [Unsubscribe](https://docs.aws.amazon.com/sns/latest/api/API_Unsubscribe.html) actions are invoked, then they will not be logged to CloudTrail\. Such as, when you choose the provided link in an email notification to confirm a pending subscription to a topic, the `ConfirmSubscription` action is invoked in unauthenticated mode\. In this example, the `ConfirmSubscription` action would not be logged to CloudTrail\.

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

## Example: Amazon SNS log file entries<a name="understanding-service-name-entries"></a>

 A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\.

The following example shows a CloudTrail log entry that demonstrates the `ListTopics`, `CreateTopic`, and `DeleteTopic` actions\.

```
{
  "Records": [
    {
      "eventVersion": "1.02",
      "userIdentity": {
       "type":"IAMUser",
       "userName":"Bob"
       "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Bob",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE"
      },
      "eventTime": "2014-09-30T00:00:00Z",
      "eventSource": "sns.amazonaws.com",
      "eventName": "ListTopics",
      "awsRegion": "us-west-2",
      "sourceIPAddress": "127.0.0.1",
      "userAgent": "aws-sdk-java/unknown-version",
      "requestParameters": {
        "nextToken": "ABCDEF1234567890EXAMPLE=="
      },
      "responseElements": null,
      "requestID": "example1-b9bb-50fa-abdb-80f274981d60",
      "eventID": "example0-09a3-47d6-a810-c5f9fd2534fe",
      "eventType": "AwsApiCall",
      "recipientAccountId": "123456789012"
    },
    {
      "eventVersion": "1.02",
      "userIdentity": {
       "type":"IAMUser",
       "userName":"Bob"
       "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Bob",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE"
      },
      "eventTime": "2014-09-30T00:00:00Z",
      "eventSource": "sns.amazonaws.com",
      "eventName": "CreateTopic",
      "awsRegion": "us-west-2",
      "sourceIPAddress": "127.0.0.1",
      "userAgent": "aws-sdk-java/unknown-version",
      "requestParameters": {
        "name": "hello"
      },
      "responseElements": {
        "topicArn": "arn:aws:sns:us-west-2:123456789012:hello-topic"
      },
      "requestID": "example7-5cd3-5323-8a00-f1889011fee9",
      "eventID": "examplec-4f2f-4625-8378-130ac89660b1",
      "eventType": "AwsApiCall",
      "recipientAccountId": "123456789012"
    },
    {
      "eventVersion": "1.02",
      "userIdentity": {
       "type":"IAMUser",
       "userName":"Bob"
       "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Bob",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE"
      },
      "eventTime": "2014-09-30T00:00:00Z",
      "eventSource": "sns.amazonaws.com",
      "eventName": "DeleteTopic",
      "awsRegion": "us-west-2",
      "sourceIPAddress": "127.0.0.1",
      "userAgent": "aws-sdk-java/unknown-version",
      "requestParameters": {
        "topicArn": "arn:aws:sns:us-west-2:123456789012:hello-topic"
      },
      "responseElements": null,
      "requestID": "example5-4faa-51d5-aab2-803a8294388d",
      "eventID": "example8-6443-4b4d-abfd-1b867280d964",
      "eventType": "AwsApiCall",
      "recipientAccountId": "123456789012"
    },
  ]
}
```