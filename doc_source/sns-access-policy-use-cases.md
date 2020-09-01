# Example cases for Amazon SNS access control<a name="sns-access-policy-use-cases"></a>

**Topics**
+ [Grant AWS account access to a topic](#sns-grant-aws-account-access-to-topic)
+ [Limit subscriptions to HTTPS](#sns-limit-subscriptions-to-https)
+ [Publish messages to an Amazon SQS queue](#sns-publish-messages-to-sqs-queue)
+ [Allow any AWS resource to publish to a topic](#sns-allow-any-aws-resource-to-publish-to-topic)
+ [Allow an Amazon S3 bucket to publish to a topic](#sns-allow-s3-bucket-to-publish-to-topic)
+ [Allow another AWS service to publish to a topic that is owned by another account](#sns-allow-specified-service-to-publish-to-topic)
+ [Allow accounts in an AWS organization to publish to a topic in a different account](#sns-allow-organization-to-publish-to-topic-in-another-account)
+ [Allow any CloudWatch alarm to publish to a topic in a different account](#sns-allow-cloudwatch-alarm-to-publish-to-topic-in-another-account)
+ [Restrict publication to an Amazon SNS topic only from a specific VPC endpoint](#sns-restrict-publication-only-from-specified-vpc-endpoint)

This section grants a few examples of typical use cases for access control\.

## Grant AWS account access to a topic<a name="sns-grant-aws-account-access-to-topic"></a>

Let's say you have a topic in the Amazon SNS system\. In the simplest case, you want to allow one or more AWS accounts access to a specific topic action \(for example, Publish\)\.

You can do this using the Amazon SNS API action `AddPermission`\. It takes a topic, a list of AWS account IDs, a list of actions, and a label, and automatically creates a new statement in the topic's access control policy\. In this case, you don't write a policy yourself, because Amazon SNS automatically generates the new policy statement for you\. You can remove the policy statement later by calling `RemovePermission` with its label\.

For example, if you called `AddPermission` on the topic arn:aws:sns:us\-east\-2:444455556666:MyTopic, with AWS account ID 1111\-2222\-3333, the `Publish` action, and the label `grant-1234-publish`, Amazon SNS would generate and insert the following access control policy statement:

```
{
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

## Limit subscriptions to HTTPS<a name="sns-limit-subscriptions-to-https"></a>

In the following example, you limit the notification delivery protocol to HTTPS\.

You need to know how to write your own policy for the topic because the Amazon SNS `AddPermission` action doesn't let you specify a protocol restriction when granting someone access to your topic\. In this case, you would write your own policy, and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example of a full policy grants the AWS account ID 1111\-2222\-3333 the ability to subscribe to notifications from a topic\.

```
{
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

## Publish messages to an Amazon SQS queue<a name="sns-publish-messages-to-sqs-queue"></a>

In this use case, you want to publish messages from your topic to your Amazon SQS queue\. Like Amazon SNS, Amazon SQS uses Amazon's access control policy language\. To allow Amazon SNS to send messages, you'll need to use the Amazon SQS action `SetQueueAttributes` to set a policy on the queue\.

Again, you'll need to know how to write your own policy because the Amazon SQS `AddPermission` action doesn't create policy statements with conditions\.

**Note**  
The example presented below is an Amazon SQS policy \(controlling access to your queue\), not an Amazon SNS policy \(controlling access to your topic\)\. The actions are Amazon SQS actions, and the resource is the Amazon Resource Name \(ARN\) of the queue\. You can determine the queue's ARN by retrieving the queue's `QueueArn` attribute with the `GetQueueAttributes` action\.

```
{
  "Statement": [{
    "Sid": "Allow-SNS-SendMessage",
    "Effect": "Allow",
    "Principal": {
      "Service": "sns.amazonaws.com"
    },
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

## Allow any AWS resource to publish to a topic<a name="sns-allow-any-aws-resource-to-publish-to-topic"></a>

In this case, you want to configure a topic's policy so that another AWS account's resource \(for example, Amazon S3 bucket, Amazon EC2 instance, or Amazon SQS queue\) can publish to your topic\. This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

In the following example statement, the topic owner in these policies is 1111\-2222\-3333 and the AWS resource owner is 4444\-5555\-6666\. The example grants the AWS account ID 4444\-5555\-6666 the ability to publish to My\-Topic from any AWS resource owned by the account\.

```
{
  "Statement": [{
    "Effect": "Allow",
    "Principal": { 
      "Service": "someservice.amazonaws.com" 
    },
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

## Allow an Amazon S3 bucket to publish to a topic<a name="sns-allow-s3-bucket-to-publish-to-topic"></a>

In this case, you want to configure a topic's policy so that another AWS account's Amazon S3 bucket can publish to your topic\. For more information about publishing notifications from Amazon S3, go to [Setting Up Notifications of Bucket Events](https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html)\. 

This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example statement uses the `SourceAccount` condition to ensure that only the Amazon S3 owner account can access the topic\. In this example, the topic owner is 1111\-2222\-3333 and the Amazon S3 owner is 4444\-5555\-6666\. The example states that any Amazon S3 bucket owned by 4444\-5555\-6666 is allowed to publish to MyTopic\.

```
{
  "Statement": [{
    "Effect": "Allow",
     "Principal": { 
      "Service": "s3.amazonaws.com" 
    },
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

## Allow another AWS service to publish to a topic that is owned by another account<a name="sns-allow-specified-service-to-publish-to-topic"></a>

You can allow another AWS service to publish to a topic that is owned by another AWS account\. Suppose that you signed into the 111122223333 account, opened Amazon SES, and created an email\. To publish notifications about this email to a Amazon SNS topic that the 444455556666 account owns, you'd create a policy like the following\. To do so, you need to provide information about the principal \(the other service\) and each resource's ownership\. The `Resource` statement provides the topic ARN, which includes the account ID of the topic owner, 444455556666\. The `"aws:SourceOwner": "111122223333"` statement specifies that your account owns the email\. 

```
{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__default_statement_ID",
      "Effect": "Allow",
      "Principal": {
        "Service": "ses.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
      "Condition": {
        "StringEquals": {
          "aws:SourceOwner": "111122223333"
        }
      }
    }
  ]
}
```

### `aws:SourceAccount` versus `aws:SourceOwner`<a name="source-account-versus-source-owner"></a>

The `aws:SourceAccount` and `aws:SourceOwner` condition keys are similar in that they both identify a resource owner\. This section describes the differences between each condition key and when to use each one when creating access policies for Amazon SNS topics\.

When you create topics from the Amazon SNS console, the default policy uses the `aws:SourceOwner` condition key to allow only the topic owner to publish and subscribe to the topic\. The value for `aws:SourceOwner` is the topic owner\. In more advanced access policies, use the condition keys as follows: 
+ Use `aws:SourceAccount` to configure conditions for other accounts for publishing and subscribing to topics\.
+ Use `aws:SourceOwner` to allow publishing requests from another AWS service\. Use the above example to specify ownership of the other service's resource and of the Amazon SNS topic\.

## Allow accounts in an AWS organization to publish to a topic in a different account<a name="sns-allow-organization-to-publish-to-topic-in-another-account"></a>

The AWS Organizations service helps you to centrally manage billing, control access and security, and share resources across your AWS accounts\. 

You can find your organization ID in the [ Organizations console](https://console.aws.amazon.com/organizations/)\. For more information, see [ Viewing details of an organization from the master account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_details.html#orgs_view_org)\. 

In this example, any AWS account in organization `myOrgId` can publish to Amazon SNS topic `MyTopic` in account `444455556666`\. The policy checks the organization ID value using the `aws:PrincipalOrgID` global condition key\.

```
{
"Statement": [{
  "Effect": "Allow",
  "Principal": {
     "AWS": "*"
  },
  "Action": "SNS:Publish",
  "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
  "Condition": {
      "StringEquals": {
            "aws:PrincipalOrgID": "myOrgId"
      }
  }
}]
```

## Allow any CloudWatch alarm to publish to a topic in a different account<a name="sns-allow-cloudwatch-alarm-to-publish-to-topic-in-another-account"></a>

In this case, any CloudWatch alarms in account `111122223333` are allowed to publish to an Amazon SNS topic in account `444455556666`\.

```
{
 "Statement": [{
    "Effect": "Allow",
    "Principal": {
       "AWS": "*"
    },
    "Action": "SNS:Publish",
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
    "Condition": {
       "ArnLike": {
          "aws:SourceArn": "arn:aws:cloudwatch:us-east-2:111122223333:alarm:*"
       }
    }
 }]
```

## Restrict publication to an Amazon SNS topic only from a specific VPC endpoint<a name="sns-restrict-publication-only-from-specified-vpc-endpoint"></a>

In this case, the topic in account 444455556666 is allowed to publish only from the VPC endpoint with the ID `vpc-1ab2c34d`\.

```
{
  "Statement": [{
    "Effect": "Deny",
    "Principal": "*",
    "Action": "SNS:Publish",
    "Resource": "arn:aws:sns:us-east-2:444455556666:MyTopic",
    "Condition": {
      "StringNotEquals": {
        "aws:sourceVpce": "vpc-1ab2c34d"
      }
    }
  }]
}
```