# Archived message format for Amazon S3 destinations<a name="firehose-archived-message-format-S3"></a>

The following example shows an Amazon SNS notification sent to an Amazon Simple Storage Service \(Amazon S3\) bucket, using indents for readability\.

**Note**  
In this example, raw message delivery is disabled for the published message\. When raw message delivery is disabled, Amazon SNS adds JSON metadata to the message, including these properties:  
`Type`
`MessageId`
`TopicArn`
`Subject`
`Timestamp`
`UnsubscribeURL`
`MessageAttributes`
For more information about raw delivery, see [Amazon SNS raw message delivery](sns-large-payload-raw-message-delivery.md)\.

```
{
    "Type": "Notification",
    "MessageId": "719a6bbf-f51b-5320-920f-3385b5e9aa56",
    "TopicArn": "arn:aws:sns:us-east-1:333333333333:my-kinesis-test-topic",     
    "Subject": "My 1st subject",
    "Message": "My 1st body",
    "Timestamp": "2020-11-26T23:48:02.032Z",
    "UnsubscribeURL": "https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:333333333333:my-kinesis-test-topic:0b410f3c-ee5e-49d8-b59b-3b4aa6d8fcf5",
    "MessageAttributes": {
        "myKey1": {
            "Type": "String",
            "Value": "myValue1"
        },
        "myKey2": {
            "Type": "String",
            "Value": "myValue2"
        }
    }
 }
```

The following example shows three SNS messages sent through an Amazon Kinesis Data Firehose delivery stream to the same Amazon S3 bucket\. Buffering is taken into account, and line breaks separate the messages\.

```
{"Type":"Notification","MessageId":"d7d2513e-6126-5d77-bbe2-09042bd0a03a","TopicArn":"arn:aws:sns:us-east-1:333333333333:my-kinesis-test-topic","Subject":"My 1st subject","Message":"My 1st body","Timestamp":"2020-11-27T00:30:46.100Z","UnsubscribeURL":"https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:313276652360:my-kinesis-test-topic:0b410f3c-ee5e-49d8-b59b-3b4aa6d8fcf5","MessageAttributes":{"myKey1":{"Type":"String","Value":"myValue1"},"myKey2":{"Type":"String","Value":"myValue2"}}}
{"Type":"Notification","MessageId":"0c0696ab-7733-5bfb-b6db-ce913c294d56","TopicArn":"arn:aws:sns:us-east-1:333333333333:my-kinesis-test-topic","Subject":"My 2nd subject","Message":"My 2nd body","Timestamp":"2020-11-27T00:31:22.151Z","UnsubscribeURL":"https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:313276652360:my-kinesis-test-topic:0b410f3c-ee5e-49d8-b59b-3b4aa6d8fcf5","MessageAttributes":{"myKey1":{"Type":"String","Value":"myValue1"}}}
{"Type":"Notification","MessageId":"816cd54d-8cfa-58ad-91c9-8d77c7d173aa","TopicArn":"arn:aws:sns:us-east-1:333333333333:my-kinesis-test-topic","Subject":"My 3rd subject","Message":"My 3rd body","Timestamp":"2020-11-27T00:31:39.755Z","UnsubscribeURL":"https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:313276652360:my-kinesis-test-topic:0b410f3c-ee5e-49d8-b59b-3b4aa6d8fcf5"}
```