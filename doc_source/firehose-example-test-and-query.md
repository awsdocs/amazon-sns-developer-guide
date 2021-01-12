# Testing and querying the configuration<a name="firehose-example-test-and-query"></a>

This page describes how to test the [message archiving and analytics example use case](firehose-example-use-case.md) by publishing a message to the Amazon SNS topic\. The instructions include an example query that you can run and adapt to your own needs\.

**To test your configuration**

1. Open the [Topics page](https://console.aws.amazon.com/sns/home#/topics) of the Amazon SNS console\.

1. Choose the **ticketTopic** topic\.

1. Choose **Publish message**\.

1. On the **Publish message to topic** page, enter the following for the message body\. Add a newline character at the end of the message\.

   ```
   {"BookingDate":"2020-12-15","BookingTime":"2020-12-15 04:15:05","Destination":"Miami","FlyingFrom":"Vancouver","TicketNumber":"abcd1234"}
   ```

   Keep all other options as their defaults\.

1. Choose **Publish message**\.

   For more information on publishing messages, see [Amazon SNS message publishing](sns-publishing.md)\.

1. After the delivery stream interval of 60 seconds, open the [Amazon Simple Storage Service \(Amazon S3\) console](https://console.aws.amazon.com/s3/home) and choose the Amazon S3 bucket that you [created initially](firehose-example-initial-resources.md)\.

   The published message appears in the bucket\.

**To query the data**

1. Open the [Amazon Athena console](https://console.aws.amazon.com/athena/home)\.

1. Run a query\.

   For example, assume that the `notifications` table in the `default` schema contains the following data:

   ```
   {"BookingDate":"2020-12-15","BookingTime":"2020-12-15 04:15:05","Destination":"Miami","FlyingFrom":"Vancouver","TicketNumber":"abcd1234"}
   {"BookingDate":"2020-12-15","BookingTime":"2020-12-15 11:30:15","Destination":"Miami","FlyingFrom":"Omaha","TicketNumber":"efgh5678"}
   {"BookingDate":"2020-12-15","BookingTime":"2020-12-15 3:30:10","Destination":"Miami","FlyingFrom":"NewYork","TicketNumber":"ijkl9012"}
   {"BookingDate":"2020-12-15","BookingTime":"2020-12-15 12:30:05","Destination":"Delhi","FlyingFrom":"Omaha","TicketNumber":"mnop3456"}
   ```

   To find the top destination, run the following query:

   ```
   SELECT destination
   FROM default.notifications
   GROUP BY destination
   ORDER BY count(*) desc
   LIMIT 1;
   ```

   To query for tickets sold during a specific date and time range, run a query like the following:

   ```
   SELECT * 
   FROM default.notifications 
   WHERE bookingtime 
     BETWEEN TIMESTAMP '2020-12-15 10:00:00' 
     AND TIMESTAMP '2020-12-15 12:00:00';
   ```

   You can adapt both sample queries for your own needs\. For more information on using Athena to run queries, see [Getting Started](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html) in the *Amazon Athena User Guide*\.

## Cleaning up<a name="firehose-example-cleanup"></a>

To avoid incurring usage charges after you're done testing, delete the following resources that you created during the tutorial:
+ Amazon SNS subscriptions
+ Amazon SNS topic
+ Amazon Simple Queue Service \(Amazon SQS\) queues
+ Amazon S3 bucket
+ Amazon Kinesis Data Firehose delivery stream
+ AWS Identity and Access Management \(IAM\) roles and policies