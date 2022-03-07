# Removing a subscription filter policy<a name="message-filtering-policy-remove"></a>

To stop filtering the messages that are sent to a subscription, remove the subscription's filter policy by overwriting it with an empty JSON body\. After you remove the policy, the subscription accepts every message that's published to it\.

## AWS Management Console<a name="message-filtering-policy-remove-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. Select a subscription and then choose **Edit**\.

1. On the **Edit *EXAMPLE1\-23bc\-4567\-d890\-ef12g3hij456*** page, expand the **Subscription filter policy** section\.

1. In the **JSON editor** field, provide an empty JSON body for your filter policy: `{}`\.

1. Choose **Save changes**\.

   Amazon SNS applies your filter policy to the subscription\.

## AWS CLI<a name="message-filtering-policy-remove-cli"></a>

To remove a filter policy with the AWS CLI, use the [https://docs.aws.amazon.com/cli/latest/reference/sns/set-subscription-attributes.html](https://docs.aws.amazon.com/cli/latest/reference/sns/set-subscription-attributes.html) command and provide an empty JSON body for the `--attribute-value` argument:

```
$ aws sns set-subscription-attributes --subscription-arn arn:aws:sns: ... --attribute-name FilterPolicy --attribute-value "{}"
```

## Amazon SNS API<a name="message-filtering-policy-remove-api"></a>

To remove a filter policy with the Amazon SNS API, make a request to the [https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html](https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html) action\. Set the `AttributeName` parameter to `FilterPolicy`, and provide an empty JSON body for the `AttributeValue` parameter\.