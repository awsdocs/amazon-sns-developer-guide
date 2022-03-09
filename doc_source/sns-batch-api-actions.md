# Amazon SNS message batching<a name="sns-batch-api-actions"></a>

## What is message batching?<a name="what-is-message-batching"></a>

An alternative to publishing messages to either Standard or FIFO topics in individual `Publish` API requests, is using the Amazon SNS `PublishBatch` API to publish up to 10 messages in a single API request\. Sending messages in batches can help you reduce the costs associated with connecting distributed applications \([A2A messaging](sns-system-to-system-messaging.md)\) or sending notifications to people \([A2P messaging](sns-user-notifications.md)\) with Amazon SNS by a factor of up to 10\. Amazon SNS has quotas on how many messages you can publish to a topic per second based on the region in which you operate\. See the [Amazon SNS endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html) page in the *AWS General Reference* guide for more information on API quotas\.

**Note**  
The total aggregate size of all messages that you send in a single `PublishBatch` API request can’t exceed 262,144 bytes \(256 KB\)\.  
The `PublishBatch` API uses the same `Publish` API action for IAM policies\.

## How does message batching work?<a name="message-batching-how-it-works"></a>

Publishing messages with the `PublishBatch` API is similar to publishing messages with the `Publish` API\. The main difference is that each message within a `PublishBatch` API request needs to be assigned a unique batch ID \(up to 80 characters\)\. This way, Amazon SNS can return individual API responses for every message within a batch to confirm that each message was either published or that a failure occurred\. For messages being published to FIFO topics, in addition to including assigning a unique batch ID, you still need to include a `MessageDeduplicationID` and `MessageGroupId` for each individual message\.

## Examples<a name="message-batching-examples"></a>

**Publishing a batch of 10 messages to a Standard topic**

```
// Imports
import com.amazonaws.services.sns.AmazonSNS;
import com.amazonaws.services.sns.model.PublishBatchRequest;
import com.amazonaws.services.sns.model.PublishBatchRequestEntry;
import com.amazonaws.services.sns.model.PublishBatchResult;
import com.amazonaws.services.sns.model.AmazonSNSException;
import java.util.List;
import java.util.stream.Collectors;

// Code
private static final int MAX_BATCH_SIZE = 10;

public static void publishBatchToTopic(AmazonSNS snsClient, String topicArn) {
    try {
        // Create the batch entries to send
        List<PublishBatchRequestEntry> entries = IntStream.range(0, MAX_BATCH_SIZE)
                .mapToObj(i -> new PublishBatchRequestEntry()
                        .withId("id" + i)
                        .withMessage("message" + i))
                .collect(Collectors.toList());

        // Create the batch request
        PublishBatchRequest request = new PublishBatchRequest()
                .withTopicArn(topicArn)
                .withPublishBatchRequestEntries(entries);

        // Publish the batch request
        PublishBatchResult publishBatchResult = snsClient.publishBatch(request);

        // Handle the successfully sent messages
        publishBatchResult.getSuccessful().forEach(publishBatchResultEntry -> {
            System.out.println("Batch Id for successful message: " + publishBatchResultEntry.getId());
            System.out.println("Message Id for successful message: " + publishBatchResultEntry.getMessageId());
        });

        // Handle the failed messages
        publishBatchResult.getFailed().forEach(batchResultErrorEntry -> {
            System.out.println("Batch Id for failed message: " + batchResultErrorEntry.getId());
            System.out.println("Error Code for failed message: " + batchResultErrorEntry.getCode());
            System.out.println("Sender Fault for failed message: " + batchResultErrorEntry.getSenderFault());
            System.out.println("Failure Message for failed message: " + batchResultErrorEntry.getMessage());
        });
    } catch (AmazonSNSException e) {
        // Handle any exceptions from the request
        System.err.println(e.getMessage());
        System.exit(1);
    }
}
```

**Publishing a batch of 10 messages to a FIFO topic**

```
// Imports
import com.amazonaws.services.sns.AmazonSNS;
import com.amazonaws.services.sns.model.PublishBatchRequest;
import com.amazonaws.services.sns.model.PublishBatchRequestEntry;
import com.amazonaws.services.sns.model.PublishBatchResult;
import com.amazonaws.services.sns.model.AmazonSNSException;
import java.util.List;
import java.util.stream.Collectors;

// Code
private static final int MAX_BATCH_SIZE = 10;

public static void publishBatchToFifoTopic(AmazonSNS snsClient, String topicArn) {
    try {
        // Create the batch entries to send
        List<PublishBatchRequestEntry> entries = IntStream.range(0, MAX_BATCH_SIZE)
                .mapToObj(i -> new PublishBatchRequestEntry()
                        .withId("id" + i)
                        .withMessage("message" + i)
                        .withMessageGroupId("groupId")
                        .withMessageDeduplicationId("deduplicationId" + i))
                .collect(Collectors.toList());

        // Create the batch request
        PublishBatchRequest request = new PublishBatchRequest()
                .withTopicArn(topicArn)
                .withPublishBatchRequestEntries(entries);

        // Publish the batch request
        PublishBatchResult publishBatchResult = snsClient.publishBatch(request);

        // Handle the successfully sent messages
        publishBatchResult.getSuccessful().forEach(publishBatchResultEntry -> {
            System.out.println("Batch Id for successful message: " + publishBatchResultEntry.getId());
            System.out.println("Message Id for successful message: " + publishBatchResultEntry.getMessageId());
            System.out.println("SequenceNumber for successful message: " + publishBatchResultEntry.getSequenceNumber());
        });

        // Handle the failed messages
        publishBatchResult.getFailed().forEach(batchResultErrorEntry -> {
            System.out.println("Batch Id for failed message: " + batchResultErrorEntry.getId());
            System.out.println("Error Code for failed message: " + batchResultErrorEntry.getCode());
            System.out.println("Sender Fault for failed message: " + batchResultErrorEntry.getSenderFault());
            System.out.println("Failure Message for failed message: " + batchResultErrorEntry.getMessage());
        });

    } catch (AmazonSNSException e) {
        // Handle any exceptions from the request
        System.err.println(e.getMessage());
        System.exit(1);
    }
}
```