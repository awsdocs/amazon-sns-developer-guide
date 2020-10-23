# Message delivery for FIFO topics<a name="fifo-message-delivery"></a>

To preserve strict message ordering, Amazon SNS restricts the set of supported delivery protocols for Amazon SNS FIFO topics\. Currently, the endpoint protocol must be Amazon SQS, with an Amazon SQS FIFO queue's Amazon Resource Name \(ARN\) as the endpoint\.

**Note**  
To fan out messages from Amazon SNS FIFO topics to AWS Lambda functions, extra steps are required\. First, subscribe Amazon SQS FIFO queues to the topic\. Then configure the queues to trigger the functions\. For more information, see the [ SQS FIFO as an event source](https://aws.amazon.com/blogs/compute/new-for-aws-lambda-sqs-fifo-as-an-event-source/) post on the *AWS Compute Blog*\. 

SNS FIFO topics can't deliver messages to customer managed endpoints, such as email addresses, mobile apps, phone numbers for text messaging \(SMS\), or HTTP\(S\) endpoints\. These endpoint types aren't guaranteed to preserve strict message ordering\. Attempts to subscribe customer managed endpoints to SNS FIFO topics result in errors\.

 SNS FIFO topics support the same message filtering capabilities as standard topics\. For more information, see [Message filtering for FIFO topics](fifo-message-filtering.md) and the [ Simplify Your Pub/Sub Messaging with Amazon SNS Message Filtering](https://aws.amazon.com/blogs/compute/simplify-pubsub-messaging-with-amazon-sns-message-filtering/) post on the *AWS Compute Blog*\.