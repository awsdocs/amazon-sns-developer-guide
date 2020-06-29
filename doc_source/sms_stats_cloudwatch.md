# Viewing Amazon CloudWatch metrics and logs for SMS deliveries<a name="sms_stats_cloudwatch"></a>

You can use Amazon CloudWatch and Amazon CloudWatch Logs to monitor your SMS message deliveries\.

**Topics**
+ [Viewing Amazon CloudWatch metrics](#sms_stats_cloudwatch_metrics)
+ [Viewing CloudWatch Logs](#sns-viewing-cloudwatch-logs)
+ [Example log for successful SMS delivery](#example-log-successful-sms-delivery)
+ [Example log for failed SMS delivery](#example-log-failed-sms-delivery)
+ [SMS delivery failure reasons](#sms_stats_delivery_fail_reasons)

## Viewing Amazon CloudWatch metrics<a name="sms_stats_cloudwatch_metrics"></a>

Amazon SNS automatically collects metrics about your SMS message deliveries and pushes them to Amazon CloudWatch\. You can use CloudWatch to monitor these metrics and create alarms to alert you when a metric crosses a threshold\. For example, you can monitor CloudWatch metrics to learn your SMS delivery rate and your month\-to\-date SMS charges\.

For information about monitoring CloudWatch metrics, setting CloudWatch alarms, and the types of metrics available, see [Monitoring Amazon SNS topics using CloudWatch](sns-monitoring-using-cloudwatch.md)\.

## Viewing CloudWatch Logs<a name="sns-viewing-cloudwatch-logs"></a>

You can collect information about successful and unsuccessful SMS message deliveries by enabling Amazon SNS to write to Amazon CloudWatch Logs\. For each SMS message that you send, Amazon SNS will write a log that includes the message price, the success or failure status, the reason for failure \(if the message failed\), the message dwell time, and other information\.

**To enable CloudWatch Logs for your SMS messages**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, set the region selector to a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Text messaging \(SMS\)** page, in the **Text messaging preferences** section, choose **Edit**\.

1. On the **Edit text messaging preferences** page, in the **Delivery status logging** section, do the following:

   1. For **Success sample rate**, specify the percentage of successful SMS deliveries for which Amazon SNS will write logs in CloudWatch Logs\. For example:
      + To write logs only for failed deliveries, set this value to 0\.
      + To write logs for 10% of your successful deliveries, set it to 10\.

      If you don't specify a percentage, Amazon SNS writes logs for all successful deliveries\.

   1. Choose **Create new service role**\.

   1. Choose **Create new roles**\.

   1. On the **SNS is requesting permission to use resources in your account** page, choose **Allow**\.

1. Choose **Save changes**\.

## Example log for successful SMS delivery<a name="example-log-successful-sms-delivery"></a>

The delivery status log for a successful SMS delivery will resemble the following example:

```
{
    "notification": {
        "messageId": "34d9b400-c6dd-5444-820d-fbeb0f1f54cf",
        "timestamp": "2016-06-28 00:40:34.558"
    },
    "delivery": {
        "phoneCarrier": "My Phone Carrier",
        "mnc": 270,
        "destination": "+1XXX5550100",
        "priceInUSD": 0.00645,
        "smsType": "Transactional",
        "mcc": 310,
        "providerResponse": "Message has been accepted by phone carrier",
        "dwellTimeMs": 599,
        "dwellTimeMsUntilDeviceAck": 1344
    },
    "status": "SUCCESS"
}
```

## Example log for failed SMS delivery<a name="example-log-failed-sms-delivery"></a>

The delivery status log for a failed SMS delivery will resemble the following example:

```
{
    "notification": {
        "messageId": "1077257a-92f3-5ca3-bc97-6a915b310625",
        "timestamp": "2016-06-28 00:40:34.559"
    },
    "delivery": {
        "mnc": 0,
        "destination": "+1XXX5550100",
        "priceInUSD": 0.00645,
        "smsType": "Transactional",
        "mcc": 0,
        "providerResponse": "Unknown error attempting to reach phone",
        "dwellTimeMs": 1420,
        "dwellTimeMsUntilDeviceAck": 1692
    },
    "status": "FAILURE"
}
```

## SMS delivery failure reasons<a name="sms_stats_delivery_fail_reasons"></a>

The reason for a failure is provided with the `providerResponse` attribute\. SMS messages might fail to deliver for the following reasons:
+ Blocked as spam by phone carrier
+ Destination is on a blocked list
+ Invalid phone number
+ Message body is invalid
+ Phone carrier has blocked this message
+ Phone carrier is currently unreachable/unavailable
+ Phone has blocked SMS
+ Phone is on a blocked list
+ Phone is currently unreachable/unavailable
+ Phone number is opted out
+ This delivery would exceed max price
+ Unknown error attempting to reach phone