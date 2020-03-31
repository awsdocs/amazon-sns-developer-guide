# Using identity\-based policies with Amazon SNS<a name="sns-using-identity-based-policies"></a>

**Topics**
+ [IAM and Amazon SNS policies together](#iam-and-sns-policies)
+ [Amazon SNS resource ARN format](#sns-arn-format)
+ [Amazon SNS API actions](#sns-api-actions)
+ [Amazon SNS policy keys](#sns-policy-keys)
+ [Example policies for Amazon SNS](#sns-example-policies)

Amazon Simple Notification Service integrates with AWS Identity and Access Management \(IAM\) so that you can specify which Amazon SNS actions a user in your AWS account can perform with Amazon SNS resources\. You can specify a particular topic in the policy\. For example, you could use variables when creating an IAM policy that grants certain users in your organization permission to use the `Publish` action with specific topics in your AWS account\. For more information, see [Policy Variables](https://docs.aws.amazon.com/IAM/latest/UserGuide/PolicyVariables.html) in the *Using IAM* guide\.

**Important**  
Using Amazon SNS with IAM doesn't change how you use Amazon SNS\. There are no changes to Amazon SNS actions, and no new Amazon SNS actions related to users and access control\.

For examples of policies that cover Amazon SNS actions and resources, see [Example policies for Amazon SNS](#sns-example-policies)\.

## IAM and Amazon SNS policies together<a name="iam-and-sns-policies"></a>

You use an IAM policy to restrict your users' access to Amazon SNS actions and topics\. An IAM policy can restrict access only to users within your AWS account, not to other AWS accounts\.

You use an Amazon SNS policy with a particular topic to restrict who can work with that topic \(for example, who can publish messages to it, who can subscribe to it, etc\.\)\. Amazon SNS policies can grant access to other AWS accounts, or to users within your own AWS account\.

To grant your users permissions for your Amazon SNS topics, you can use IAM policies, Amazon SNS policies, or both\. For the most part, you can achieve the same results with either\. For example, the following diagram shows an IAM policy and an Amazon SNS policy that are equivalent\. The IAM policy allows the Amazon SNS `Subscribe` action for the topic called topic\_xyz in your AWS account\. The IAM policy is attached to the users Bob and Susan \(which means that Bob and Susan have the permissions stated in the policy\)\. The Amazon SNS policy likewise grants Bob and Susan permission to access `Subscribe` for topic\_xyz\.

![\[Equivalent IAM and Amazon SNS policies\]](http://docs.aws.amazon.com/sns/latest/dg/images/SNS_EquivalentPolicies.png)

**Note**  
The preceding example shows simple policies with no conditions\. You could specify a particular condition in either policy and get the same result\.

There is one difference between AWS IAM and Amazon SNS policies: The Amazon SNS policy system lets you grant permission to other AWS accounts, whereas the IAM policy doesn't\. 

It's up to you how you use both of the systems together to manage your permissions, based on your needs\. The following examples show how the two policy systems work together\.

**Example 1**  
In this example, both an IAM policy and an Amazon SNS policy apply to Bob\. The IAM policy grants him permission for `Subscribe` on any of the AWS account's topics, whereas the Amazon SNS policy grants him permission to use `Publish` on a specific topic \(topic\_xyz\)\. The following diagram illustrates the concept\.  

![\[IAM and Amazon SNS policies for Bob\]](http://docs.aws.amazon.com/sns/latest/dg/images/SNS_UnionOfPolicies.png)
If Bob were to send a request to subscribe to any topic in the AWS account, the IAM policy would allow the action\. If Bob were to send a request to publish a message to topic\_xyz, the Amazon SNS policy would allow the action\.

**Example 2**  
In this example, we build on example 1 \(where Bob has two policies that apply to him\)\. Let's say that Bob publishes messages to topic\_xyz that he shouldn't have, so you want to entirely remove his ability to publish to topics\. The easiest thing to do is to add an IAM policy that denies him access to the `Publish` action on all topics\. This third policy overrides the Amazon SNS policy that originally gave him permission to publish to topic\_xyz, because an explicit deny always overrides an allow \(for more information about policy evaluation logic, see [Evaluation logic](sns-access-policy-language-evaluation-logic.md)\)\. The following diagram illustrates the concept\.  

![\[The "Deny" policy overrides the Amazon SNS policy\]](http://docs.aws.amazon.com/sns/latest/dg/images/SNS_DenyOverride.png)

For examples of policies that cover Amazon SNS actions and resources, see [Example policies for Amazon SNS](#sns-example-policies)\. For more information about writing Amazon SNS policies, go to the [technical documentation for Amazon SNS](http://aws.amazon.com/documentation/sns/)\.

## Amazon SNS resource ARN format<a name="sns-arn-format"></a>

For Amazon SNS, topics are the only resource type you can specify in a policy\. The following is the Amazon Resource Name \(ARN\) format for topics\.

```
arn:aws:sns:region:account_ID:topic_name
```

For more information about ARNs, go to [ARNs](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_Identifiers.html#Identifiers_ARNs) in *IAM User Guide*\.

**Example**  
The following is an ARN for a topic named MyTopic in the us\-east\-2 region, belonging to AWS account 123456789012\.   

```
arn:aws:sns:us-east-2:123456789012:MyTopic
```

**Example**  
If you had a topic named MyTopic in each of the different Regions that Amazon SNS supports, you could specify the topics with the following ARN\.   

```
arn:aws:sns:*:123456789012:MyTopic
```

You can use \* and ? wildcards in the topic name\. For example, the following could refer to all the topics created by Bob that he has prefixed with `bob_`\.

```
arn:aws:sns:*:123456789012:bob_*
```

As a convenience to you, when you create a topic, Amazon SNS returns the topic's ARN in the response\.

## Amazon SNS API actions<a name="sns-api-actions"></a>

In an IAM policy, you can specify any actions that Amazon SNS offers\. However, the `ConfirmSubscription` and `Unsubscribe` actions do not require authentication, which means that even if you specify those actions in a policy, IAM won't restrict users' access to those actions\.

Each action you specify in a policy must be prefixed with the lowercase string `sns:`\. To specify all Amazon SNS actions, for example, you would use `sns:*`\. For a list of the actions, go to the [Amazon Simple Notification Service API Reference](https://docs.aws.amazon.com/sns/latest/api/)\. 

## Amazon SNS policy keys<a name="sns-policy-keys"></a>

Amazon SNS implements the following AWS\-wide policy keys, plus some service\-specific keys\.

For a list of condition keys supported by each AWS service, see [Actions, Resources, and Condition Keys for AWS Services](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_actions-resources-contextkeys.html) in the *IAM User Guide*\. For a list of condition keys that can be used in multiple AWS services, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

Amazon SNS uses the following service\-specific keys\. Use these keys in policies that restrict access to `Subscribe` requests\.
+ **sns:endpoint—**The URL, email address, or ARN from a `Subscribe` request or a previously confirmed subscription\. Use with string conditions \(see [Example policies for Amazon SNS](#sns-example-policies)\) to restrict access to specific endpoints \(for example, \*@yourcompany\.com\)\.
+ **sns:protocol—**The `protocol` value from a `Subscribe` request or a previously confirmed subscription\. Use with string conditions \(see [Example policies for Amazon SNS](#sns-example-policies)\) to restrict publication to specific delivery protocols \(for example, https\)\.

## Example policies for Amazon SNS<a name="sns-example-policies"></a>

This section shows several simple policies for controlling user access to Amazon SNS\.

**Note**  
In the future, Amazon SNS might add new actions that should logically be included in one of the following policies, based on the policy’s stated goals\. 

**Example 1: Allow a group to create and manage topics**  
In this example, we create a policy that grants access to `CreateTopic`, `ListTopics`, `SetTopicAttributes`, and `DeleteTopic`\.  

```
{
  "Statement": [{
    "Effect": "Allow",
    "Action": ["sns:CreateTopic", "sns:ListTopics", "sns:SetTopicAttributes", "sns:DeleteTopic"],
    "Resource": "*"
  }]
}
```

**Example 2: Allow the IT group to publish messages to a particular topic**  
In this example, we create a group for IT, and assign a policy that grants access to `Publish` on the specific topic of interest\.  

```
{
  "Statement": [{
    "Effect": "Allow",
    "Action": "sns:Publish",
    "Resource": "arn:aws:sns:*:123456789012:MyTopic"
  }]
}
```

**Example 3: Give users in the AWS account ability to subscribe to topics**  
In this example, we create a policy that grants access to the `Subscribe`action, with string matching conditions for the `sns:Protocol` and `sns:Endpoint` policy keys\.  

```
{
  "Statement": [{
    "Effect": "Allow",
    "Action": ["sns:Subscribe"],
    "Resource": "*",
    "Condition": {
      "StringLike": {
        "SNS:Endpoint": "*@example.com"
      },
      "StringEquals": {
        "sns:Protocol": "email"
      }
    }
  }]
}
```

**Example 4: Allow a partner to publish messages to a particular topic**  
You can use an Amazon SNS policy or an IAM policy to allow a partner to publish to a specific topic\. If your partner has an AWS account, it might be easier to use an Amazon SNS policy\. However, anyone in the partner's company who possesses the AWS security credentials could publish messages to the topic\. This example assumes that you want to limit access to a particular person \(or application\)\. To do this you need to treat the partner like a user within your own company, and use a IAM policy instead of an Amazon SNS policy\.  
For this example, we create a group called WidgetCo that represents the partner company; we create a user for the specific person \(or application\) at the partner company who needs access; and then we put the user in the group\.   
We then attach a policy that grants the group `Publish` access on the specific topic named *WidgetPartnerTopic*\.   
We also want to prevent the WidgetCo group from doing anything else with topics, so we add a statement that denies permission to any Amazon SNS actions other than `Publish` on any topics other than WidgetPartnerTopic\. This is necessary only if there's a broad policy elsewhere in the system that grants users wide access to Amazon SNS\.   

```
{
  "Statement": [{
      "Effect": "Allow",
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:*:123456789012:WidgetPartnerTopic"
    },
    {
      "Effect": "Deny",
      "NotAction": "sns:Publish",
      "NotResource": "arn:aws:sns:*:123456789012:WidgetPartnerTopic"
    }
  ]
}
```