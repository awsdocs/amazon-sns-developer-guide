# Email notifications<a name="sns-email-notifications"></a>

Amazon SNS supports the delivery of notifications to email addresses that are subscribed to SNS topics\. For more information, see [Subscribing to an Amazon SNS topic](sns-create-subscribe-endpoint-to-topic.md)\. 

**Notes**  
Email subscriptions must be confirmed before you can receive notifications from the topic\.
You can't customize the body of the email message\. The email delivery feature is intended to provide internal system alerts, not marketing messages\.
Email delivery throughput is throttled according to [Amazon SNS quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html#limits_sns)\.