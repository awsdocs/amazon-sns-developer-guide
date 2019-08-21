# Monitoring Amazon SNS Topics Using CloudWatch<a name="sns-monitoring-using-cloudwatch"></a>

Amazon SNS and CloudWatch are integrated so you can collect, view, and analyze metrics for every active Amazon SNS notification\. Once you have configured CloudWatch for Amazon SNS, you can gain better insight into the performance of your Amazon SNS topics, push notifications, and SMS deliveries\. For example, you can set an alarm to send you an email notification if a specified threshold is met for an Amazon SNS metric, such as `NumberOfNotificationsFailed`\. For a list of all the metrics that Amazon SNS sends to CloudWatch, see [Amazon SNS Metrics](#SNS_metricscollected)\. For more information about Amazon SNS push notifications, see [Using Amazon SNS for User Notifications with a Mobile Application as a Subscriber \(Mobile Push\)](sns-mobile-application-as-subscriber.md) 

The metrics you configure with CloudWatch for your Amazon SNS topics are automatically collected and pushed to CloudWatch every five minutes\. These metrics are gathered on all topics that meet the CloudWatch guidelines for being active\. A topic is considered active by CloudWatch for up to six hours from the last activity \(that is, any API call\) on the topic\. 

**Note**  
There is no charge for the Amazon SNS metrics reported in CloudWatch; they are provided as part of the Amazon SNS service\.

## Access CloudWatch Metrics for Amazon SNS<a name="SNS_ViewMetrics"></a>

You can monitor metrics for Amazon SNS using the CloudWatch console, CloudWatch's own command line interface \(CLI\), or programmatically using the CloudWatch API\. The following procedures show you how to access the metrics using these different options\. 

**To view metrics using the CloudWatch console**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **View Metrics**\.

1. For **Viewing**, choose either **SNS: Topic Metrics**, **SNS: Push Notifications by Application**, **SNS: Push Notifications by Application and Platform**, or **SNS: Push Notifications by Platform** to show the available metrics\.

1. Choose a specific item to see more detail, such as a graph of the data collected\. For example, the following graph of the selected metric, **NumberOfMessagesPublished**, shows the average number of published Amazon SNS messages for a five\-minute period throughout the time range of 6 hours\.   
![\[\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-cloudwatch-metric.png)

**To access metrics from the CloudWatch CLI**
+ Call `[mon\-get\-stats](https://docs.aws.amazon.com/AmazonCloudWatch/latest/cli/cli-mon-get-stats.html)`\. You can learn more about this and other metrics\-related functions in the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/)\.

**To access metrics from the CloudWatch API**
+ Call `[GetMetricStatistics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricStatistics.html)`\. You can learn more about this and other metrics\-related functions in the [Amazon CloudWatch API Reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/)\.

## Set CloudWatch Alarms for Amazon SNS Metrics<a name="SNS_AlarmMetrics"></a>

CloudWatch also allows you to set alarms when a threshold is met for a metric\. For example, you could set an alarm for the metric, **NumberOfNotificationsFailed**, so that when your specified threshold number is met within the sampling period, then an email notification would be sent to inform you of the event\.

**To set alarms using the CloudWatch console**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1.  Choose **Alarms**, and then choose the **Create Alarm** button\. This launches the **Create Alarm** wizard\. 

1.  Scroll through the Amazon SNS metrics to locate the metric you want to place an alarm on\. Select the metric to create an alarm on and choose **Continue**\. 

1. Fill in the **Name**, **Description**, **Threshold**, and **Time** values for the metric, and then choose **Continue**\. 

1. Choose **Alarm** as the alarm state\. If you want CloudWatch to send you an email when the alarm state is reached, choose either an existing Amazon SNS topic or choose **Create New Email Topic**\. If you choose **Create New Email Topic**, you can set the name and email addresses for a new topic\. This list will be saved and appear in the drop\-down box for future alarms\. Choose **Continue**\. 
**Note**  
If you use **Create New Email Topic** to create a new Amazon SNS topic, the email addresses must be verified before they will receive notifications\. Emails are sent only when the alarm enters an alarm state\. If this alarm state change happens before the email addresses are verified, they will not receive a notification\. 

1. At this point, the **Create Alarm** wizard gives you a chance to review the alarm you’re about to create\. If you need to make any changes, you can use the **Edit** links on the right\. Once you are satisfied, choose **Create Alarm**\. 

For more information about using CloudWatch and alarms, see the [CloudWatch Documentation](https://aws.amazon.com/documentation/cloudwatch)\.

## Amazon SNS Metrics<a name="SNS_metricscollected"></a>

Amazon SNS sends the following metrics to CloudWatch\.


| Metric | Description | 
| --- | --- | 
|  `NumberOfMessagesPublished`  |  The number of messages published to your Amazon SNS topics\. Units: *Count* Valid Statistics: Sum  | 
|  `NumberOfNotificationsDelivered`  |  The number of messages successfully delivered from your Amazon SNS topics to subscribing endpoints\. For a delivery attempt to succeed, the endpoint's subscription must accept the message\. A subscription accepts a message if a\.\) it lacks a filter policy or b\.\) its filter policy includes attributes that match those assigned to the message\. If the subscription rejects the message, the delivery attempt isn't counted for this metric\. Units: *Count* Valid Statistics: Sum  | 
|  `NumberOfNotificationsFailed`  |  The number of messages that Amazon SNS failed to deliver\.  For Amazon SQS, email, SMS, or mobile push endpoints, the metric increments by 1 when Amazon SNS stops attempting message deliveries\. For HTTP or HTTPS endpoints, the metric includes every failed delivery attempt, including retries that follow the initial attempt\. For all other endpoints, the count increases by 1 when the message fails to deliver \(regardless of the number of attempts\)\. This metric does not include messages that were rejected by subscription filter policies\. You can control the number of retries for HTTP endpoints\. For more information, see [Setting Amazon SNS Delivery Retry Policies for HTTP/HTTPS Endpoints](DeliveryPolicies.md)\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut |  The number of messages that were rejected by subscription filter policies\. A filter policy rejects a message when the message attributes don't match the policy attributes\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut\-NoMessageAttributes |  The number of messages that were rejected by subscription filter policies because the messages have no attributes\. Units: *Count* Valid Statistics: Sum, Average  | 
| NumberOfNotificationsFilteredOut\-InvalidAttributes |  The number of messages that were rejected by subscription filter policies because the messages' attributes are invalid – for example, because the attribute JSON is incorrectly formatted\. Units: *Count* Valid Statistics: Sum, Average  | 
|  `PublishSize`  |  The size of messages published\. Units: *Bytes* Valid Statistics: Minimum, Maximum, Average and Count  | 
| SMSMonthToDateSpentUSD |  The charges you have accrued since the start of the current calendar month for sending SMS messages\. You can set an alarm for this metric to know when your month\-to\-date charges are close to the monthly SMS spend limit for your account\. When Amazon SNS determines that sending an SMS message would incur a cost that exceeds this limit, it stops publishing SMS messages within minutes\. For information about setting your monthly SMS spend limit, or for information about requesting a spend limit increase with AWS, see [Setting SMS Messaging Preferences](sms_preferences.md)\. Units: *USD* Valid Statistics: Maximum  | 
|  `SMSSuccessRate`  |  The rate of successful SMS message deliveries\. Units: *Count* Valid Statistics: Sum, Average, Data Samples  | 

## Dimensions for Amazon Simple Notification Service Metrics<a name="sns-metric-dimensions"></a>

Amazon Simple Notification Service sends the following dimensions to CloudWatch\.


|  Dimension  |  Description  | 
| --- | --- | 
|  Application  |  Filters on application objects, which represent an app and device registered with one of the supported push notification services, such as APNS and FCM\.  | 
|  Application,Platform  |  Filters on application and platform objects, where the platform objects are for the supported push notification services, such as APNS and FCM\.  | 
| Country |  Filters on the destination country or region of an SMS message\. The country or region is represented by its ISO 3166\-1 alpha\-2 code\.  | 
|  Platform  |  Filters on platform objects for the push notification services, such as APNS and FCM\.  | 
|  TopicName  |  Filters on Amazon SNS topic names\.  | 
| SMSType |  Filters on the message type of SMS message\. Can be *promotional* or *transactional*\.  | 