# Create and publish to a FIFO Amazon SNS topic using an AWS SDK<a name="example_sns_PublishFifoTopic_section"></a>

The following code example shows how to create and publish to a FIFO Amazon SNS topic\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 1\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/java/example_code/sns#code-examples)\. 
Create a FIFO topic and FIFO queues\. Subscribe the queues to the topic\.  

```
// Create API clients

AWSCredentialsProvider credentials = getCredentials();

AmazonSNS sns = new AmazonSNSClient(credentials);
AmazonSQS sqs = new AmazonSQSClient(credentials);

// Create FIFO topic

Map<String, String> topicAttributes = new HashMap<String, String>();

topicAttributes.put("FifoTopic", "true");
topicAttributes.put("ContentBasedDeduplication", "false");

String topicArn = sns.createTopic(
    new CreateTopicRequest()
        .withName("PriceUpdatesTopic.fifo")
        .withAttributes(topicAttributes)
).getTopicArn();

// Create FIFO queues

Map<String, String> queueAttributes = new HashMap<String, String>();

queueAttributes.put("FifoQueue", "true");

// Disable content-based deduplication because messages published with the same body
// might carry different attributes that must be processed independently.
// The price management system uses the message attributes to define whether a given
// price update applies to the wholesale application or to the retail application.
queueAttributes.put("ContentBasedDeduplication", "false");

String wholesaleQueueUrl = sqs.createQueue(
    new CreateQueueRequest()
        .withName("WholesaleQueue.fifo")
        .withAttributes(queueAttributes)
).getQueueUrl();

String retailQueueUrl = sqs.createQueue(
    new CreateQueueRequest()
        .withName("RetailQueue.fifo")
        .withAttributes(queueAttributes)
).getQueueUrl();

// Subscribe FIFO queues to FIFO topic, setting required permissions

String wholesaleSubscriptionArn =
    Topics.subscribeQueue(sns, sqs, topicArn, wholesaleQueueUrl);

String retailSubscriptionArn =
    Topics.subscribeQueue(sns, sqs, topicArn, retailQueueUrl);
```
Set a filter policy on each subscription so that each subscriber application receives only the price updates that it needs\.  

```
// Set the Amazon SNS subscription filter policies

SNSMessageFilterPolicy wholesalePolicy = new SNSMessageFilterPolicy();
wholesalePolicy.addAttribute("business", "wholesale");
wholesalePolicy.apply(sns, wholesaleSubscriptionArn);

SNSMessageFilterPolicy retailPolicy = new SNSMessageFilterPolicy();
retailPolicy.addAttribute("business", "retail");
retailPolicy.apply(sns, retailSubscriptionArn);
```
Compose and publish a message that updates the wholesale price\.  

```
// Publish message to FIFO topic

String subject = "Price Update";
String payload = "{\"product\": 214, \"price\": 79.99}";
String groupId = "PID-214";
String dedupId = UUID.randomUUID().toString();
String attributeName = "business";
String attributeValue = "wholesale";

Map<String, MessageAttributeValue> attributes = new HashMap<>();

attributes.put(
    attributeName,
    new MessageAttributeValue()
        .withDataType("String")
        .withStringValue(attributeValue));

sns.publish(
    new PublishRequest()
        .withTopicArn(topicArn)
        .withSubject(subject)
        .withMessage(payload)
        .withMessageGroupId(groupId);
        .withMessageDeduplicationId(dedupId)
        .withMessageAttributes(attributes);
```

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SNS with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.