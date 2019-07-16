# Example Cases for Amazon SNS Access Control<a name="AccessPolicyLanguage_UseCases_Sns"></a>

**Topics**
+ [Allowing AWS account Access to a Topic](#AccessPolicyLanguage_UseCase1_Sns)
+ [Limiting Subscriptions to HTTPS](#AccessPolicyLanguage_UseCase2_Sns)
+ [Publishing to an Amazon SQS Queue](#AccessPolicyLanguage_UseCase3_Sns)
+ [Allowing Any AWS Resource to Publish to a Topic](#AccessPolicyLanguage_UseCase4_Sns)
+ [Allowing an Amazon S3 Bucket to Publish to a Topic](#AccessPolicyLanguage_UseCase5_Sns)

This section gives a few examples of typical use cases for access control\.

## Allowing AWS account Access to a Topic<a name="AccessPolicyLanguage_UseCase1_Sns"></a>

Let's say you have a topic in the Amazon SNS system\. In the simplest case, you want to allow one or more AWS accounts access to a specific topic action \(e\.g\., Publish\)\.

You can do this using the Amazon SNS API action `AddPermission`\. It takes a topic, a list of AWS account IDs, a list of actions, and a label, and automatically creates a new statement in the topic's access control policy\. In this case, you don't write a policy yourself, because Amazon SNS automatically generates the new policy statement for you\. You can remove the policy statement later by calling `RemovePermission` with its label\.

For example, if you called `AddPermission` on the topic arn:aws:sns:us\-east\-1:444455556666:MyTopic, with AWS account ID 1111\-2222\-3333, the `Publish` action, and the label `give-1234-publish`, Amazon SNS would generate and insert the following access control policy statement:

```
 1. {
 2. 		    "Version":"2012-10-17",
 3. 		    "Id":"AWSAccountTopicAccess",
 4. 		    "Statement" :[
 5. 		        {
 6. 		            "Sid":"give-1234-publish",
 7. 		            "Effect":"Allow",           
 8. 		            "Principal" :{
 9. 		                "AWS":"111122223333"
10. 		             },
11. 		            "Action":["sns:Publish"],
12. 		            "Resource":"arn:aws:sns:us-east-1:444455556666:MyTopic"
13. 		        }
14. 		    ]
15. 		}
```

Once this statement is added, the user with AWS account 1111\-2222\-3333 can publish messages to the topic\.

## Limiting Subscriptions to HTTPS<a name="AccessPolicyLanguage_UseCase2_Sns"></a>

In the following example, you limit the notification delivery protocol to HTTPS\.

You need to know how to write your own policy for the topic because the Amazon SNS `AddPermission` action doesn't let you specify a protocol restriction when granting someone access to your topic\. In this case, you would write your own policy, and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example of a full policy gives the AWS account ID 1111\-2222\-3333 the ability to subscribe to notifications from a topic\.

```
 1. {   
 2. 		    "Version":"2012-10-17",
 3. 		    "Id":"SomePolicyId",
 4. 		    "Statement" :[
 5. 		        {
 6. 		            "Sid":"Statement1",
 7. 		            "Effect":"Allow",           
 8. 		            "Principal" :{
 9. 		                "AWS":"111122223333"
10. 		              },
11. 		            "Action":["sns:Subscribe"],
12. 		            "Resource": "arn:aws:sns:us-east-1:444455556666:MyTopic",
13. 		            "Condition" :{
14. 		                "StringEquals" :{
15. 		                    "sns:Protocol":"https"
16. 		                 }
17. 		            }   
18. 		        }
19. 		    ]
20. 		}
```

## Publishing to an Amazon SQS Queue<a name="AccessPolicyLanguage_UseCase3_Sns"></a>

In this use case, you want to publish messages from your topic to your Amazon SQS queue\. Like Amazon SNS, Amazon SQS uses Amazon's access control policy language\. To allow Amazon SNS to send messages, you'll need to use the Amazon SQS action `SetQueueAttributes` to set a policy on the queue\.

Again, you'll need to know how to write your own policy because the Amazon SQS `AddPermission` action doesn't create policy statements with conditions\.

Note that the example presented below is an Amazon SQS policy \(controlling access to your queue\), not an Amazon SNS policy \(controlling access to your topic\)\. The actions are Amazon SQS actions, and the resource is the Amazon Resource Name \(ARN\) of the queue\. You can determine the queue's ARN by retrieving the queue's `QueueArn` attribute with the `GetQueueAttributes` action\.

```
 1. {   
 2. 		    "Version":"2012-10-17",
 3. 		    "Id":"MyQueuePolicy",
 4. 		    "Statement" :[
 5. 		        {
 6. 			        "Sid":"Allow-SNS-SendMessage",
 7. 			        "Effect":"Allow",           
 8. 			        "Principal" :"*",
 9. 			        "Action":["sqs:SendMessage"],
10. 			        "Resource": "arn:aws:sqs:us-east-1:444455556666:MyQueue",
11. 			        "Condition" :{
12. 			            "ArnEquals" :{
13. 				            "aws:SourceArn":"arn:aws:sns:us-east-1:444455556666:MyTopic"
14. 		                 }
15. 		            }
16. 		        }
17. 		    ]
18. 		}
```

This policy uses the `aws:SourceArn` condition to restrict access to the queue based on the source of the message being sent to the queue\. You can use this type of policy to allow Amazon SNS to send messages to your queue only if the messages are coming from one of your own topics\. In this case, you specify a particular one of your topics, whose ARN is arn:aws:sns:us\-east\-1:444455556666:MyTopic\.

The preceding policy is an example of the Amazon SQS policy you could write and add to a specific queue\. It would grant Amazon SNS and other AWS products access\. Amazon SNS gives a default policy to all newly created topics\. The default policy gives all other AWS products access to your topic\. This default policy uses an `aws:SourceArn` condition to ensure that AWS products access your topic only on behalf of AWS resources you own\.

## Allowing Any AWS Resource to Publish to a Topic<a name="AccessPolicyLanguage_UseCase4_Sns"></a>

In this case, you want to configure a topic's policy so that another AWS account's resource \(e\.g\., Amazon S3 bucket, Amazon EC2 instance, or Amazon SQS queue\) can publish to your topic\. This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

In the following example statement, the topic owner in these policies is 1111\-2222\-3333 and the AWS resource owner is 4444\-5555\-6666\. The example gives the AWS account ID 4444\-5555\-6666 the ability to publish to My\-Topic from any AWS resource owned by the account\.

```
 1. {
 2. 		    "Version":"2012-10-17",
 3. 		    "Id":"MyAWSPolicy",
 4. 		    "Statement" :[
 5. 		        {
 6. 		            "Sid":"My-statement-id",
 7. 		            "Effect":"Allow",
 8. 		            "Principal" :"*",
 9. 		            "Action":"sns:Publish",
10. 		            "Resource":"arn:aws:sns:us-east-1:111122223333:My-Topic",
11. 		            "Condition":{
12. 		               "StringEquals":{
13. 		                  "AWS:SourceAccount":"444455556666"
14. 		                }
15. 		            }
16. 		        }
17. 		    ]
18. 		}
```

## Allowing an Amazon S3 Bucket to Publish to a Topic<a name="AccessPolicyLanguage_UseCase5_Sns"></a>

 In this case, you want to configure a topic's policy so that another AWS account's Amazon S3 bucket can publish to your topic\. For more information about publishing notifications from Amazon S3, go to [Setting Up Notifications of Bucket Events](https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html)\. 

This example assumes that you write your own policy and then use the `SetTopicAttributes` action to set the topic's `Policy` attribute to your new policy\.

The following example statement uses the `ArnLike` condition to make sure the ARN of the resource making the request \(the `AWS:SourceARN`\) is an Amazon S3 ARN\. You could use a similar condition to restrict the permission to a set of Amazon S3 buckets, or even to a specific bucket\. In this example, the topic owner is 1111\-2222\-3333 and the Amazon S3 owner is 4444\-5555\-6666\. The example states that any Amazon S3 bucket owned by 4444\-5555\-6666 is allowed to publish to My\-Topic\.

```
 1. {
 2. 		    "Version":"2012-10-17",
 3. 		    "Id":"MyAWSPolicy",
 4. 		    "Statement" :[
 5. 		        {
 6. 		            "Sid":"My-statement-id",
 7. 		            "Effect":"Allow",
 8. 		            "Principal" :"*",
 9. 		            "Action":"sns:Publish",
10. 		            "Resource":"arn:aws:sns:us-east-1:111122223333:My-Topic",
11. 		            "Condition":{
12. 		                "StringEquals":{ "AWS:SourceAccount":"444455556666" } ,
13. 		                "ArnLike": {"AWS:SourceArn": "arn:aws:s3:*:*:*" }
14. 		            }
15. 		        }
16. 		    ]
17. 		}
```