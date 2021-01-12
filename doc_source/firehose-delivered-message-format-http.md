# Delivered message format for HTTP destinations<a name="firehose-delivered-message-format-http"></a>

The following is an example HTTP POST request body from Amazon SNS that an Amazon Kinesis Data Firehose delivery stream can send to the HTTP endpoint\. The SNS notification is encoded as a base64 payload in the `records` property\.

**Note**  
In this example, raw message delivery is disabled for the published message\. For more information about raw delivery, see [Amazon SNS raw message delivery](sns-large-payload-raw-message-delivery.md)\.

```
"body": {
    "requestId": "ebc9e8b2-fce3-4aef-a8f1-71698bf8175f",
    "timestamp": 1606255960435,
    "records": [
      {
        "data": "eyJUeXBlIjoiTm90aWZpY2F0aW9uIiwiTWVzc2FnZUlkIjoiMjFkMmUzOGQtMmNhYi01ZjYxLTliYTItYmJiYWFhYzg0MGY2IiwiVG9waWNBcm4iOiJhcm46YXdzOnNuczp1cy1lYXN0LTE6MTExMTExMTExMTExOm15LXRvcGljIiwiTWVzc2FnZSI6IlNhbXBsZSBtZXNzYWdlIGZvciBBbWF6b24gS2luZXNpcyBEYXRhIEZpcmVob3NlIGVuZHBvaW50cyIsIlRpbWVzdGFtcCI6IjIwMjAtMTEtMjRUMjI6MDc6MzEuNjY3WiIsIlVuc3Vic2NyaWJlVVJMIjoiaHR0cHM6Ly9zbnMudXMtZWFzdC0xLmFtYXpvbmF3cy5jb20vP0FjdGlvbj1VbnN1YnNjcmliZSZTdWJzY3JpcHRpb25Bcm49YXJuOmF3czpzbnM6MTExMTExMTExMTExOm15LXRvcGljOjAxYjY5MTJjLTAwNzAtNGQ4Yi04YjEzLTU1NWJmYjc2ZTdkNCJ9"
      }
    ]
  }
```