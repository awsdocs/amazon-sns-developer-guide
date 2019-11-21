# Example Cases for Amazon SNS Access Control<a name="sns-access-policy-use-cases"></a>

**Topics**
+ [Grant AWS Account Access to a Topic](#sns-grant-aws-account-access-to-topic)
+ [Limit Subscriptions to HTTPS](#sns-limit-subscriptions-to-https)
+ [Publish Messages to an Amazon SQS Queue](#sns-publish-messages-to-sqs-queue)
+ [Allow Any AWS Resource to Publish to a Topic](#sns-allow-any-aws-resource-to-publish-to-topic)
+ [Allow an Amazon S3 Bucket to Publish to a Topic](#sns-allow-s3-bucket-to-publish-to-topic)
+ [Allow a CloudWatch Alarm in an AWS Account to Publish to an Amazon SNS Topic in a Different AWS Account](#sns-allow-cloudwatch-alarm-to-publish-to-topic-in-another-account)

This section grants a few examples of typical use cases for access control\.

## Grant AWS Account Access to a Topic<a name="sns-grant-aws-account-access-to-topic"></a>

Let's say you have a topic in the Amazon SNS system\. In the simplest case, you want to allow one or more AWS accounts access to a specific topic action \(for example, Publish\)\.

You can do this using the Amazon SNS API action `AddPermission`\. It takes a topic, a list of AWS account IDs, a list of actions, and a label, and automatically creates a new statement in the topic's access control policy\. In this case, you don't write a policy yourself, because Amazon SNS automatically generates the new policy statement for you\. You can remove the policy statement later by calling `RemovePermission` with its label\.

For example, if you called `AddPermission` on the topic arn:aws:sns:us\-east\-2:444455556666:MyTopic, with AWS account ID 1111\-2222\-3333, the `Publish` action, and the label `grant-1234-publish`, Amazon SNS would generate and insert the following access control policy statement:

```
{
  "Version": "2012-10-17",
  "Id": "AWSAccountTopicAccess",
  "Statement": [{
    "Sid": "grant-1234-publish",
    "Effect": "Allow",
    "Principal": {
      "AWS": "111122223333"
    },
    "Action": ["sns:Publish"],
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic"
  }]
}
```

Once this statement is added, the user with AWS account 1111\-2222\-3333 can publish messages to the topic\.

## Limit Subscriptions to HTTPS<a name="sns-limit-subscriptions-to-https"></a>

In the following example, you limit the notification delivery protocol to HTTPS\.

You need to know how to write your own policy for the topic because the Amazon SNS `AddPermission` action doesn't let you specify a protocol restriction when granting someone access to your topic\. In this case, you would write your own policy, and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example of a full policy grants the AWS account ID 1111\-2222\-3333 the ability to subscribe to notifications from a topic\.

```
{
  "Version": "2012-10-17",
  "Id": "SomePolicyId",
  "Statement": [{
    "Sid": "Statement1",
    "Effect": "Allow",
    "Principal": {
      "AWS": "111122223333"
    },
    "Action": ["sns:Subscribe"],
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
    "Condition": {
      "StringEquals": {
        "sns:Protocol": "https"
      }
    }
  }]
}
```

## Publish Messages to an Amazon SQS Queue<a name="sns-publish-messages-to-sqs-queue"></a>

In this use case, you want to publish messages from your topic to your Amazon SQS queue\. Like Amazon SNS, Amazon SQS uses Amazon's access control policy language\. To allow Amazon SNS to send messages, you'll need to use the Amazon SQS action `SetQueueAttributes` to set a policy on the queue\.

Again, you'll need to know how to write your own policy because the Amazon SQS `AddPermission` action doesn't create policy statements with conditions\.

**Note**  
The example presented below is an Amazon SQS policy \(controlling access to your queue\), not an Amazon SNS policy \(controlling access to your topic\)\. The actions are Amazon SQS actions, and the resource is the Amazon Resource Name \(ARN\) of the queue\. You can determine the queue's ARN by retrieving the queue's `QueueArn` attribute with the `GetQueueAttributes` action\.

```
{
  "Version": "2012-10-17",
  "Id": "MyQueuePolicy",
  "Statement": [{
    "Sid": "Allow-SNS-SendMessage",
    "Effect": "Allow",
    "Principal": "*",
    "Action": ["sqs:SendMessage"],
    "Resource": "arn:aws:sqs:us-east-2:444455556666:MyQueue",
    "Condition": {
      "ArnEquals": {
        "aws:SourceArn": "arn:aws:sns:us-east-2:444455556666:MyTopic"
      }
    }
  }]
}
```

This policy uses the `aws:SourceArn` condition to restrict access to the queue based on the source of the message being sent to the queue\. You can use this type of policy to allow Amazon SNS to send messages to your queue only if the messages are coming from one of your own topics\. In this case, you specify a particular one of your topics, whose ARN is arn:aws:sns:us\-east\-2:444455556666:MyTopic\.

The preceding policy is an example of the Amazon SQS policy you could write and add to a specific queue\. It would grant access to Amazon SNS and other AWS services\. Amazon SNS grants a default policy to all newly created topics\. The default policy grants access to your topic to all other AWS services\. This default policy uses an `aws:SourceArn` condition to ensure that AWS services access your topic only on behalf of AWS resources you own\.

## Allow Any AWS Resource to Publish to a Topic<a name="sns-allow-any-aws-resource-to-publish-to-topic"></a>

In this case, you want to configure a topic's policy so that another AWS account's resource \(for example, Amazon S3 bucket, Amazon EC2 instance, or Amazon SQS queue\) can publish to your topic\. This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

In the following example statement, the topic owner in these policies is 1111\-2222\-3333 and the AWS resource owner is 4444\-5555\-6666\. The example grants the AWS account ID 4444\-5555\-6666 the ability to publish to My\-Topic from any AWS resource owned by the account\.

**Note**  
If you publish messages directly \(rather than having an AWS resource publish messages on your behalf\), a policy in which you specify an empty `Principal` *and *use `AWS:SourceAccount` as a condition will not work\.

```
{
  "Version": "2012-10-17",
  "Id": "MyAWSPolicy",
  "Statement": [{
    "Sid": "My-statement-id",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "sns:Publish",
    "Resource": "arn:aws:sns:us-east-2:111122223333:MyTopic",
    "Condition": {
      "StringEquals": {
        "AWS:SourceAccount": "444455556666"
      }
    }
  }]
}
```

## Allow an Amazon S3 Bucket to Publish to a Topic<a name="sns-allow-s3-bucket-to-publish-to-topic"></a>

In this case, you want to configure a topic's policy so that another AWS account's Amazon S3 bucket can publish to your topic\. For more information about publishing notifications from Amazon S3, go to [Setting Up Notifications of Bucket Events](https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html)\. 

This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example statement uses the `ArnLike` condition to make sure the ARN of the resource making the request \(the `AWS:SourceARN`\) is an Amazon S3 ARN\. You could use a similar condition to restrict the permission to a set of Amazon S3 buckets, or even to a specific bucket\. In this example, the topic owner is 1111\-2222\-3333 and the Amazon S3 owner is 4444\-5555\-6666\. The example states that any Amazon S3 bucket owned by 4444\-5555\-6666 is allowed to publish to My\-Topic\.

```
{
  "Version": "2012-10-17",
  "Id": "MyAWSPolicy",
  "Statement": [{
    "Sid": "My-statement-id",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "sns:Publish",
    "Resource": "arn:aws:sns:us-east-2:111122223333:MyTopic",
    "Condition": {
      "StringEquals": {
        "AWS:SourceAccount": "444455556666"
      },
      "ArnLike": {
        "AWS:SourceArn": "arn:aws:s3:*:*:*"
      }
    }
  }]
}
```

## Allow a CloudWatch Alarm in an AWS Account to Publish to an Amazon SNS Topic in a Different AWS Account<a name="sns-allow-cloudwatch-alarm-to-publish-to-topic-in-another-account"></a>

In this case, the CloudWatch alarm in account `111122223333` is allowed to publish to an Amazon SNS topic in account `444455556666`\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "SNS:Publish",
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:aws:cloudwatch:us-east-2:111122223333:alarm:MyAlarm"
      }
    }
  }]
}
```