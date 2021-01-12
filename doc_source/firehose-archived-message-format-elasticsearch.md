# Archived message format in Amazon ES indices<a name="firehose-archived-message-format-elasticsearch"></a>

The following example shows an Amazon SNS notification sent to an Amazon Elasticsearch Service \(Amazon ES\) index named `my-index`\. This index has a time filter field on the `Timestamp` field\. The SNS notification is placed in the `_source` property of the payload\.

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
  "_index": "my-index",
  "_type": "_doc",
  "_id": "49613100963111323203250405402193283794773886550985932802.0",
  "_version": 1,
  "_score": null,
  "_source": {
    "Type": "Notification",
    "MessageId": "bf32e294-46e3-5dd5-a6b3-bad65162e136",
    "TopicArn": "arn:aws:sns:us-east-1:111111111111:my-topic",
    "Subject": "Sample subject",
    "Message": "Sample message",
    "Timestamp": "2020-12-02T22:29:21.189Z",
    "UnsubscribeURL": "https://sns.us-east-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us-east-1:111111111111:my-topic:b5aa9bc1-9c3d-452b-b402-aca2cefc63c9",
    "MessageAttributes": {
      "my_attribute": {
        "Type": "String",
        "Value": "my_value"
      }
    }
  },
  "fields": {
    "Timestamp": [
      "2020-12-02T22:29:21.189Z"
    ]
  },
  "sort": [
    1606948161189
  ]
}
```