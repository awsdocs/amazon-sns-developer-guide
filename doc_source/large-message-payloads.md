# Publishing large messages with Amazon SNS and Amazon S3<a name="large-message-payloads"></a>

To publish large Amazon SNS messages, you can use the [The Amazon SNS Extended Client Library for Java](https://github.com/awslabs/amazon-sns-java-extended-client-lib/)\. This library is useful for messages that are larger than the current maximum of 256 KB, up to maximum of 2 GB\. The libary saves actual payload to an Amazon S3 bucket and publishes the reference of the stored Amazon S3 object to the topic\. Subscribed Amazon SQS queues can use the [Amazon SQS Extended Client Library for Java](https://github.com/awslabs/amazon-sqs-java-extended-client-lib) to de\-reference and retrieve payloads from Amazon S3\. Other endpoints, such as Lambda, can use the [Payload Offloading Java Common Library for AWS](https://github.com/awslabs/payload-offloading-java-common-lib-for-aws) to de\-reference and retrieve the payload\.

## Prerequisites<a name="prereqs-sns-extended-client-library"></a>

The following are the prerequisites for using the Amazon SNS Extended Client Library for Java:
+ An AWS SDK\.

  The example on this page uses the AWS Java SDK\. To install and set up the SDK, see [Set up the AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-install.html) in the *AWS SDK for Java Developer Guide*\.
+ An AWS account with the proper credentials\. 

  To create an AWS account, navigate to the [AWS home page](https://aws.amazon.com/), and then choose **Create an AWS Account**\. Follow the instructions\.

  For information about credentials, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) in the *AWS SDK for Java Developer Guide*\.
+ Java 8 or better\. 
+ [The Amazon SNS Extended Client Library for Java](https://github.com/awslabs/amazon-sns-java-extended-client-lib/) \(also available from [Maven](https://maven.apache.org/)\)\. 

### Configuring message storage<a name="large-message-configure-storage"></a>

The Amazon SNS Extended Payload library uses on the Payload Offloading Java Common Library for AWS for message storage and retrieval\. You can configure the following Amazon S3 [message storage options](https://github.com/awslabs/amazon-sns-java-extended-client-lib/blob/main/src/main/java/software/amazon/sns/SNSExtendedClientConfiguration.java):
+ Custom message sizes threshold – Messages with payloads and attributes that exceed this size are automatically stored in Amazon S3\.
+ `alwaysThroughS3` flag – Set this value to `true` to force all message payloads to be stored in Amazon S3\. For example:

  ```
  SNSExtendedClientConfiguration snsExtendedClientConfiguration = new 
    SNSExtendedClientConfiguration() .withPayloadSupportEnabled(s3Client, BUCKET_NAME).withAlwaysThroughS3(true);
  ```
+ Custom KMS key – The key to use for server\-side encryption in your Amazon S3 bucket\.
+ Bucket name – The name of the Amazon S3 bucket for storing message payloads\. 

## Example: Publishing messages to Amazon SNS with payload stored in Amazon S3<a name="example-s3-large-payloads"></a>

The following shows an example of how to do the following:
+ Create a sample topic and queue\.
+ Subscribe the queue to receive messages from the topic\.
+ Publish a test message\.

The message payload is stored in Amazon S3 and the reference to it is published\. The Amazon SQS Extended Client is used to receive the message\.

```
import com.amazon.sqs.javamessaging.AmazonSQSExtendedClient;
import com.amazon.sqs.javamessaging.ExtendedClientConfiguration;
import com.amazonaws.regions.Region;
import com.amazonaws.regions.Regions;
import com.amazonaws.services.s3.AmazonS3;
import com.amazonaws.services.s3.AmazonS3ClientBuilder;
import com.amazonaws.services.sns.AmazonSNS;
import com.amazonaws.services.sns.AmazonSNSClientBuilder;
import com.amazonaws.services.sns.model.CreateTopicRequest;
import com.amazonaws.services.sns.model.PublishRequest;
import com.amazonaws.services.sns.model.SetSubscriptionAttributesRequest;
import com.amazonaws.services.sns.util.Topics;
import com.amazonaws.services.sqs.AmazonSQS;
import com.amazonaws.services.sqs.AmazonSQSClientBuilder;
import com.amazonaws.services.sqs.model.CreateQueueRequest;
import com.amazonaws.services.sqs.model.ReceiveMessageResult;
import software.amazon.sns.AmazonSNSExtendedClient;
import software.amazon.sns.SNSExtendedClientConfiguration;

public class Example {

    public static void main(String[] args) {
        final String BUCKET_NAME = "extended-client-bucket";
        final String TOPIC_NAME = "extended-client-topic";
        final String QUEUE_NAME = "extended-client-queue";
        final Regions region = Regions.DEFAULT_REGION;

        //Message threshold controls the maximum message size that will be allowed to be published
        //through SNS using the extended client. Payload of messages exceeding this value will be stored in
        //S3. The default value of this parameter is 256 KB which is the maximum message size in SNS (and SQS).
        final int EXTENDED_STORAGE_MESSAGE_SIZE_THRESHOLD = 32;

        //Initialize SNS, SQS and S3 clients
        final AmazonSNS snsClient = AmazonSNSClientBuilder.standard().withRegion(region).build();
        final AmazonSQS sqsClient = AmazonSQSClientBuilder.standard().withRegion(region).build();
        final AmazonS3 s3Client = AmazonS3ClientBuilder.standard().withRegion(region).build();

        //Create bucket, topic, queue and subscription
        s3Client.createBucket(BUCKET_NAME);
        final String topicArn = snsClient.createTopic(
                new CreateTopicRequest().withName(TOPIC_NAME)
        ).getTopicArn();
        final String queueUrl = sqsClient.createQueue(
                new CreateQueueRequest().withQueueName(QUEUE_NAME)
        ).getQueueUrl();
        final String subscriptionArn = Topics.subscribeQueue(
                snsClient, sqsClient, topicArn, queueUrl
        );

        //To read message content stored in S3 transparently through SQS extended client,
        //set the RawMessageDelivery subscription attribute to TRUE
        final SetSubscriptionAttributesRequest subscriptionAttributesRequest = new SetSubscriptionAttributesRequest();
        subscriptionAttributesRequest.setSubscriptionArn(subscriptionArn);
        subscriptionAttributesRequest.setAttributeName("RawMessageDelivery");
        subscriptionAttributesRequest.setAttributeValue("TRUE");
        snsClient.setSubscriptionAttributes(subscriptionAttributesRequest);

        //Initialize SNS extended client
        //PayloadSizeThreshold triggers message content storage in S3 when the threshold is exceeded
        //To store all messages content in S3, use AlwaysThroughS3 flag
        final SNSExtendedClientConfiguration snsExtendedClientConfiguration = new SNSExtendedClientConfiguration()
                .withPayloadSupportEnabled(s3Client, BUCKET_NAME)
                .withPayloadSizeThreshold(EXTENDED_STORAGE_MESSAGE_SIZE_THRESHOLD);
        final AmazonSNSExtendedClient snsExtendedClient = new AmazonSNSExtendedClient(snsClient, snsExtendedClientConfiguration);

        //Publish message via SNS with storage in S3
        final String message = "This message is stored in S3 as it exceeds the threshold of 32 bytes set above.";
        snsExtendedClient.publish(topicArn, message);

        //Initialize SQS extended client
        final ExtendedClientConfiguration sqsExtendedClientConfiguration = new ExtendedClientConfiguration()
                .withPayloadSupportEnabled(s3Client, BUCKET_NAME);
        final AmazonSQSExtendedClient sqsExtendedClient =
                new AmazonSQSExtendedClient(sqsClient, sqsExtendedClientConfiguration);

        //Read the message from the queue
        final ReceiveMessageResult result = sqsExtendedClient.receiveMessage(queueUrl);
        System.out.println("Received message is " + result.getMessages().get(0).getBody());
    }
}
```

## Other endpoint protocols<a name="large-payloads-other-protocols"></a>

Both the Amazon SNS and Amazon SQS libraries use the [Payload Offloading Java Common Library for AWS](https://github.com/awslabs/payload-offloading-java-common-lib-for-aws) to store and retrieve message payloads with Amazon S3\. Any Java\-enabled endpoint \(for example, an HTTPS endpoint that's implemented in Java\) can use the same library to de\-reference the message content\.

Endpoints that can't use the Payload Offloading Java Common Library for AWS can still publish messages with payloads stored in Amazon S3\. The following is an example of an Amazon S3 reference that is published by the above code example:

```
[
  "software.amazon.payloadoffloading.PayloadS3Pointer",
  {
    "s3BucketName": "extended-client-bucket",
    "s3Key": "xxxx-xxxxx-xxxxx-xxxxxx"
  }
]
```