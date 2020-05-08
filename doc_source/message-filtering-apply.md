# Tutorial: Applying a subscription filter policy<a name="message-filtering-apply"></a>

You can apply a filter policy to an Amazon SNS subscription using the Amazon SNS console\. Or, to apply policies programmatically, you can use the Amazon SNS API, the AWS Command Line Interface \(AWS CLI\), or any AWS SDK that supports Amazon SNS, such as the AWS SDK for Java\.

**Important**  
AWS services such as IAM and Amazon SNS use a distributed computing model called eventual consistency\. Additions or changes to a subscription filter policy require up to 15 minutes to fully take effect\. 

## AWS Management Console<a name="message-filtering-apply-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. Select a subscription and then choose **Edit**\.

1. On the **Edit *EXAMPLE1\-23bc\-4567\-d890\-ef12g3hij456*** page, expand the **Subscription filter policy** section\.

1. In the **JSON editor** field, provide the JSON body of your filter policy\.

1. Choose **Save changes**\.

   Amazon SNS applies your filter policy to the subscription\.

## AWS CLI<a name="message-filtering-apply-cli"></a>

To apply a filter policy with the AWS Command Line Interface \(AWS CLI\), use the [https://docs.aws.amazon.com/cli/latest/reference/sns/set-subscription-attributes.html](https://docs.aws.amazon.com/cli/latest/reference/sns/set-subscription-attributes.html) command, as shown in the following example: 

```
$ aws sns set-subscription-attributes --subscription-arn arn:aws:sns: ... --attribute-name FilterPolicy --attribute-value "{\"store\":[\"example_corp\"],\"event\":[\"order_placed\"]}"
```

For the `--attribute-name` option, specify `FilterPolicy`\. For `--attribute-value`, specify your JSON policy\. 

To provide valid JSON for your policy, enclose the attribute names and values in double quotes\. You must also enclose the entire policy argument in quotes\. To avoid escaping quotes, you can use single quotes to enclose the policy and double quotes to enclose the JSON names and values, as shown in the example\.

To verify that your filter policy was applied, use the `get-subscription-attributes` command\. The attributes in the terminal output should show your filter policy for the `FilterPolicy` key, as shown in the following example:

```
$ aws sns get-subscription-attributes --subscription-arn arn:aws:sns: ...
{
    "Attributes": {
        "Endpoint": "endpoint . . .", 
        "Protocol": "https",
        "RawMessageDelivery": "false", 
        "EffectiveDeliveryPolicy": "delivery policy . . .",
        "ConfirmationWasAuthenticated": "true", 
        "FilterPolicy": "{\"store\": [\"example_corp\"], \"event\": [\"order_placed\"]}", 
        "Owner": "111122223333", 
        "SubscriptionArn": "arn:aws:sns: . . .", 
        "TopicArn": "arn:aws:sns: . . ."
    }
}
```

## AWS SDK for Java<a name="message-filtering-apply-sdks"></a>

The following examples show how to apply filter policies using the Amazon SNS clients that are provided by the AWS SDKs\.

------
#### [ AWS SDK for Java ]

To apply a filter policy with the AWS SDK for Java, use the [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sns/AmazonSNSClient.html#setSubscriptionAttributes-com.amazonaws.services.sns.model.SetSubscriptionAttributesRequest-](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sns/AmazonSNSClient.html#setSubscriptionAttributes-com.amazonaws.services.sns.model.SetSubscriptionAttributesRequest-) method of the `AmazonSNS` client\. Provide a [https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sns/model/SetSubscriptionAttributesRequest.html](https://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/sns/model/SetSubscriptionAttributesRequest.html) object as the argument, as shown in the following example:

```
AmazonSNS snsClient = AmazonSNSClientBuilder.defaultClient();
String filterPolicyString = "{\"store\":[\"example_corp\"],\"event\":[\"order_placed\"]}";
SetSubscriptionAttributesRequest request =
        new SetSubscriptionAttributesRequest(subscriptionArn, "FilterPolicy", filterPolicyString);
snsClient.setSubscriptionAttributes(request);
```

To initialize the `SetSubscriptionAttributesRequest` object, provide the following arguments:
+ `subscriptionArn` – The Amazon Resource Name \(ARN\) of the subscription to which the policy is applied\.
+ `attributeName` – Must be `"FilterPolicy"`\.
+ `attributeValue` – Your JSON filter policy as a string\. Because you must enclose the string policy in double quotes, remember to escape the double quotes that enclose the attribute names and values, as in `\"store\"`\.

The `SetSubscriptionAttributesRequest` class accepts the filter policy as a string\. If you want to define your policy as a Java collection, create a map that associates each attribute name with a list of values\. To assign the policy to a subscription, you first produce a string version of the policy from the contents of the map\. You then pass the string as the `attributeValue` argument to `SetSubscriptionAttributesRequest`\. 

------
#### [ AWS SDK for \.NET ]

To apply a filter policy with the AWS SDK for \.NET, use the [ `SetSubscriptionAttributes`](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/SNS/MSNSSetSubscriptionAttributesSetSubscriptionAttributesRequest.html) method of the `AmazonSNS` client\. Provide a [ `SetSubscriptionAttributesRequest`](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/SNS/TSetSubscriptionAttributesRequest.html) object as the argument, as shown in the following example:

```
AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient();
String filterPolicyString = "{\"store\":[\"example_corp\"],\"event\":[\"order_placed\"]}";
SetSubscriptionAttributesRequest request = new SetSubscriptionAttributesRequest(subscriptionArn, "FilterPolicy", filterPolicyString);
snsClient.setSubscriptionAttributes(request);
```

To initialize the `SetSubscriptionAttributesRequest` object, provide the following arguments:
+ `subscriptionArn` – The Amazon Resource Name \(ARN\) of the subscription to which the policy is applied\.
+ `attributeName` – Must be `"FilterPolicy"`\.
+ `attributeValue` – Your JSON filter policy as a string\. Because you must enclose the string policy in double quotes, remember to escape the double quotes that enclose the attribute names and values, as in `\"store\"`\.

The `SetSubscriptionAttributesRequest` class accepts the filter policy as a string\. If you want to define your policy as a C\# collection, create a dictionary that associates each attribute name with a list of values\. To assign the policy to a subscription, you first produce a string version of the policy from the contents of the dictionary\. You then pass the string as the `attributeValue` argument to `SetSubscriptionAttributesRequest`\. 

------

## Amazon SNS API<a name="message-filtering-apply-api"></a>

To apply a filter policy with the Amazon SNS API, make a request to the [https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html](https://docs.aws.amazon.com/sns/latest/api/API_SetSubscriptionAttributes.html) action\. Set the `AttributeName` parameter to `FilterPolicy`, and set the `AttributeValue` parameter to your filter policy JSON\.

## AWS CloudFormation<a name="message-filtering-apply-cloudformation"></a>

To apply a filter policy using AWS CloudFormation, use a JSON or YAML template to create a AWS CloudFormation stack\. For more information, see the [`FilterPolicy` property](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-sns-subscription.html#cfn-sns-subscription-filterpolicy) of the `AWS::SNS::Subscription` resource in the *AWS CloudFormation User Guide* and the [example AWS CloudFormation template](https://github.com/aws-samples/aws-sns-samples/blob/master/templates/SNS-Subscription-Attributes-Tutorial-CloudFormation.template)\.

1. Sign in to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.

1. Choose **Create Stack**\.

1. On the **Select Template** page, choose **Upload a template to Amazon S3**, choose the file, and choose **Next**\.

1. On the **Specify Details** page, do the following:

   1. For **Stack Name**, type `MyFilterPolicyStack`\.

   1. For **myHttpEndpoint**, type the HTTP endpoint to be subscribed to your topic\.
**Tip**  
If you don't have an HTTP endpoint, create one\.

1. On the **Options** page, choose **Next**\.

1. On the **Review** page, choose **Create\.**