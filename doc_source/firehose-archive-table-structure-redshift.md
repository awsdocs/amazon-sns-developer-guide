# Archive table structure for Amazon Redshift destinations<a name="firehose-archive-table-structure-redshift"></a>

For Amazon Redshift endpoints, published Amazon SNS messages are archived as rows in a table\. The following is an example\.

**Note**  
In this example, raw message delivery is disabled for the published message\. When raw message delivery is disabled, Amazon SNS adds JSON metadata to the message, including these properties:  
`Type`
`MessageId`
`TopicArn`
`Subject`
`Message`
`Timestamp`
`UnsubscribeURL`
`MessageAttributes`
For more information about raw delivery, see [Amazon SNS raw message delivery](sns-large-payload-raw-message-delivery.md)\.  
Although Amazon SNS adds properties to the message using the capitalization shown in this list, column names in Amazon Redshift tables appear in all lowercase characters\. To transform the JSON metadata for the Amazon Redshift endpoint, you can use the SQL `COPY` command\. For more information, see [Copy from JSON examples](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#r_COPY_command_examples-copy-from-json) and [Load from JSON data using the 'auto ignorecase' option](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY_command_examples.html#copy-from-json-examples-using-auto-ignorecase) in the *Amazon Redshift Database Developer Guide*\.


|  type  |  messageid  |  topicarn  |  subject  |  message  |  timestamp  |  unsubscribeurl  |  messageattributes  | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
|  Notification  |  ea544832\-a0d8\-581d\-9275\-108243c46103  |  arn:aws:sns:us\-east\-1:111111111111:my\-topic  |  Sample subject  |  Sample message  |  2020\-12\-02T00:33:32\.272Z  |  https://sns\.us\-east\-1\.amazonaws\.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us\-east\-1:111111111111:my\-topic:326deeeb\-cbf4\-45da\-b92b\-ca77a247813b  |  \{\\"my\_attribute\\":\{\\"Type\\":\\"String\\",\\"Value\\":\\"my\_value\\"\}\}  | 
|  Notification  |  ab124832\-a0d8\-581d\-9275\-108243c46114  |  arn:aws:sns:us\-east\-1:111111111111:my\-topic  |  Sample subject 2  |  Sample message 2  |  2020\-12\-03T00:18:11\.129Z  |  https://sns\.us\-east\-1\.amazonaws\.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us\-east\-1:111111111111:my\-topic:326deeeb\-cbf4\-45da\-b92b\-ca77a247813b  |  \{\\"my\_attribute2\\":\{\\"Type\\":\\"String\\",\\"Value\\":\\"my\_value\\"\}\}  | 
|  Notification  |  ce644832\-a0d8\-581d\-9275\-108243c46125  |  arn:aws:sns:us\-east\-1:111111111111:my\-topic  |  Sample subject 3  |  Sample message 3  |  2020\-12\-09T00:08:44\.405Z  |  https://sns\.us\-east\-1\.amazonaws\.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:us\-east\-1:111111111111:my\-topic:326deeeb\-cbf4\-45da\-b92b\-ca77a247813b  |  \{\\"my\_attribute3\\":\{\\"Type\\":\\"String\\",\\"Value\\":\\"my\_value\\"\}\}  | 

For more information about fanning out notifications to Amazon Redshift endpoints, see [Amazon Redshift destinations](firehose-redshift-destinations.md)\.