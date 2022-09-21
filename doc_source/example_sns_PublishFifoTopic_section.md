# Create and publish to a FIFO Amazon SNS topic using an AWS SDK<a name="example_sns_PublishFifoTopic_section"></a>

The following code example shows how to create and publish to a FIFO Amazon SNS topic\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
Create a FIFO topic and FIFO queues\. Subscribe the queues to the topic\.  

```
    public static void main(String[] args) {

        final String usage = "\n" +
            "Usage: " +
            "    <topicArn>\n\n" +
            "Where:\n" +
            "   fifoTopicName - The name of the FIFO topic. \n\n" +
            "   fifoQueueARN - The ARN value of a SQS FIFO queue. You can get this value from the AWS Management Console. \n\n";

        if (args.length != 2) {
            System.out.println(usage);
            System.exit(1);
        }

        String fifoTopicName = "PriceUpdatesTopic3.fifo";
        String fifoQueueARN = "arn:aws:sqs:us-east-1:814548047983:MyPriceSQS.fifo";
        SnsClient snsClient = SnsClient.builder()
            .region(Region.US_EAST_1)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();

        createFIFO(snsClient, fifoTopicName, fifoQueueARN);
    }

    public static void createFIFO(SnsClient snsClient, String topicName, String queueARN) {

        try {
            // Create a FIFO topic by using the SNS service client.
            Map<String, String> topicAttributes = new HashMap<>();
            topicAttributes.put("FifoTopic", "true");
            topicAttributes.put("ContentBasedDeduplication", "false");

            CreateTopicRequest topicRequest = CreateTopicRequest.builder()
                .name(topicName)
                .attributes(topicAttributes)
                .build();

            CreateTopicResponse response = snsClient.createTopic(topicRequest);
            String topicArn = response.topicArn();
            System.out.println("The topic ARN is"+topicArn);

            // Subscribe to the endpoint by using the SNS service client.
            // Only Amazon SQS FIFO queues can receive notifications from an Amazon SNS FIFO topic.
            SubscribeRequest subscribeRequest = SubscribeRequest.builder()
                .topicArn(topicArn)
                .endpoint(queueARN)
                .protocol("sqs")
                .build();

            snsClient.subscribe(subscribeRequest);
            System.out.println("The topic is subscribed to the queue.");

            // Compose and publish a message that updates the wholesale price.
            String subject = "Price Update";
            String payload = "{\"product\": 214, \"price\": 79.99}";
            String groupId = "PID-214";
            String dedupId = UUID.randomUUID().toString();
            String attributeName = "business";
            String attributeValue = "wholesale";

            MessageAttributeValue msgAttValue = MessageAttributeValue.builder()
                .dataType("String")
                .stringValue(attributeValue)
                .build();

            Map<String, MessageAttributeValue> attributes = new HashMap<>();
            attributes.put(attributeName, msgAttValue);
            PublishRequest pubRequest = PublishRequest.builder()
                .topicArn(topicArn)
                .subject(subject)
                .message(payload)
                .messageGroupId(groupId)
                .messageDeduplicationId(dedupId)
                .messageAttributes(attributes)
                .build();

            snsClient.publish(pubRequest);
            System.out.println("Message was published to "+topicArn);

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SNS with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.