# Viewing daily SMS usage reports<a name="sms_stats_usage"></a>

You can monitor your SMS deliveries by subscribing to daily usage reports from Amazon SNS\. For each day that you send at least one SMS message, Amazon SNS delivers a usage report as a CSV file to the specified Amazon S3 bucket\. It takes 24 hours for the SMS usage report to be available in the S3 bucket\. 

**Topics**
+ [Daily usage report information](#daily_usage_info)
+ [Subscribing to daily usage reports](#subscribe-to-daily-usage-reports)

## Daily usage report information<a name="daily_usage_info"></a>

The usage report includes the following information for each SMS message that you send from your account\.

 Note that the report does not include messages that are sent to recipients who have opted out\.
+ Time of publication for message \(in UTC\)
+ Message ID
+ Destination phone number
+ Message type
+ Delivery status
+ Message price \(in USD\)
+ Part number \(a message is split into multiple parts if it is too long for a single message\)
+ Total number of parts

**Note**  
If Amazon SNS did not receive the part number, we set it's value to zero\.

## Subscribing to daily usage reports<a name="subscribe-to-daily-usage-reports"></a>

To subscribe to daily usage reports, you must create an Amazon S3 bucket with the appropriate permissions\.

**To create an Amazon S3 bucket for your daily usage reports**

1. Sign in to the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.

1. Choose **Create Bucket**\.

1. For **Bucket Name**, we recommend that you enter a name that is unique for your account and your organization\. For example, use the pattern `<my-bucket-prefix>-<account_id>-<org-id>`\. 

   For information about conventions and restrictions for bucket names, see [Rules for Bucket Naming](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.

1. Choose **Create**\.

1. In the **All Buckets** table, choose the bucket and choose **Properties**\.

1. In the **Permissions** section, choose **Add bucket policy**\.

1. In the **Bucket Policy Editor** window, provide a policy that allows the Amazon SNS service principal to write to your bucket\. For an example, see [Example bucket policy](#example_bucket_policy)\.

   If you use the example policy, remember to replace *my\-s3\-bucket* with the bucket name that you chose in Step 3\.

1. Choose **Save**\.

**To subscribe to daily usage reports**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Text messaging \(SMS\)** page, in the **Text messaging preferences** section, choose **Edit**\.  
![\[Text messaging preferences section\]](http://docs.aws.amazon.com/sns/latest/dg/images/daily-usage-report1.png)

1. On the **Edit text messaging preferences** page, in the **Details** section, specify the **Amazon S3 bucket name for usage reports**\.  
![\[Details section of the Edit text messaging preferences page\]](http://docs.aws.amazon.com/sns/latest/dg/images/daily-usage-report2.png)

1. Choose **Save changes**\.

### Example bucket policy<a name="example_bucket_policy"></a>

The following policy allows the Amazon SNS service principal to perform the `s3:PutObject` and `s3:GetBucketLocation` actions\. You can use this example when you create an Amazon S3 bucket to receive daily SMS usage reports from Amazon SNS\.

```
{
  "Statement": [{
    "Sid": "AllowPutObject",
    "Effect": "Allow",
    "Principal": {
      "Service": "sns.amazonaws.com"
    },
    "Action": "s3:PutObject",
    "Resource": "arn:aws:s3:::my-s3-bucket/*"
  }, {
    "Sid": "AllowGetBucketLocation",
    "Effect": "Allow",
    "Principal": {
      "Service": "sns.amazonaws.com"
    },
    "Action": "s3:GetBucketLocation",
    "Resource": "arn:aws:s3:::my-s3-bucket"
  }]
}
```

### Example daily usage report<a name="example_report"></a>

After you subscribe to daily usage reports, each day, Amazon SNS puts a CSV file with usage data in the following location:

```
<my-s3-bucket>/SMSUsageReports/<region>/YYYY/MM/DD/00x.csv.gz
```

Each file can contain up to 50,000 records\. If the records for a day exceed this quota, Amazon SNS will add multiple files\.

The following shows an example report:

```
PublishTimeUTC,MessageId,DestinationPhoneNumber,MessageType,DeliveryStatus,PriceInUSD,PartNumber,TotalParts
2016-05-10T03:00:29.476Z,96a298ac-1458-4825-a7eb-7330e0720b72,1XXX5550100,Promotional,Message has been accepted by phone carrier,0.90084,0,1
2016-05-10T03:00:29.561Z,1e29d394-d7f4-4dc9-996e-26412032c344,1XXX5550100,Promotional,Message has been accepted by phone carrier,0.34322,0,1
2016-05-10T03:00:30.769Z,98ba941c-afc7-4c51-ba2c-56c6570a6c08,1XXX5550100,Transactional,Message has been accepted by phone carrier,0.27815,0,1
```