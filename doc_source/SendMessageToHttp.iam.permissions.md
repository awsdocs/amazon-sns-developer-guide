# Step 5: Give users permissions to publish to the topic \(optional\)<a name="SendMessageToHttp.iam.permissions"></a>

By default, the topic owner has permissions to publish the topic\. To enable other users or applications to publish to the topic, you should use AWS Identity and Access Management \(IAM\) to give publish permission to the topic\. For more information about giving permissions for Amazon SNS actions to IAM users, see [Using identity\-based policies with Amazon SNS](sns-using-identity-based-policies.md)\.

There are two ways to control access to a topic:
+ Add a policy to an IAM user or group\. The simplest way to give users permissions to topics is to create a group and add the appropriate policy to the group and then add users to that group\. It's much easier to add and remove users from a group than to keep track of which policies you set on individual users\.
+ Add a policy to the topic\. If you want to give permissions to a topic to another AWS account, the only way you can do that is by adding a policy that has as its principal the AWS account you want to give permissions to\.

You should use the first method for most cases \(apply policies to groups and manage permissions for users by adding or removing the appropriate users to the groups\)\. If you need to give permissions to a user in another account, use the second method\.

If you added the following policy to an IAM user or group, you would give that user or members of that group permission to perform the `sns:Publish` action on the topic MyTopic\.

```
{
  "Statement":[{
    "Sid":"AllowPublishToMyTopic",
    "Effect":"Allow",
    "Action":"sns:Publish",
    "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```

The following example policy shows how to give another account permissions to a topic\.

**Note**  
When you give another AWS account access to a resource in your account, you are also giving IAM users who have admin\-level access \(wildcard access\) permissions to that resource\. All other IAM users in the other account are automatically denied access to your resource\. If you want to give specific IAM users in that AWS account access to your resource, the account or an IAM user with admin\-level access must delegate permissions for the resource to those IAM users\. For more information about cross\-account delegation, see [Enabling Cross\-Account Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/Delegation.html) in the *Using IAM Guide*\.

If you added the following policy to a topic MyTopic in account 123456789012, you would give account 111122223333 permission to perform the `sns:Publish` action on that topic\.

```
{
  "Statement":[{
    "Sid":"Allow-publish-to-topic",
    "Effect":"Allow",
      "Principal":{
        "AWS":"111122223333"
      },
    "Action":"sns:Publish",
    "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
  }]
}
```