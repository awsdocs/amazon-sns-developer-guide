# Applying a subscription filter policy<a name="message-filtering-apply"></a>

You can apply a filter policy to an Amazon SNS subscription using the Amazon SNS console\. Or, to apply policies programmatically, you can use the Amazon SNS API, the AWS Command Line Interface \(AWS CLI\), or any AWS SDK that supports Amazon SNS\.

**Important**  
AWS services such as IAM and Amazon SNS use a distributed computing model called eventual consistency\. Additions or changes to a subscription filter policy require up to 15 minutes to fully take effect\. 

## AWS Management Console<a name="message-filtering-apply-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Subscriptions**\.

1. Select a subscription and then choose **Edit**\.

1. On the **Edit** page, expand the **Subscription filter policy** section\.

1. In the **JSON editor** field, provide the **JSON body** of your filter policy\.

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

## AWS SDKs<a name="message-filtering-apply-sdks"></a>

The following code examples show how to set an Amazon SNS filter policy\.

------
#### [ Java ]

**SDK for Java 2\.x**  
  

```
    public static void usePolicy(SnsClient snsClient,  String subscriptionArn) {

        try {
            SNSMessageFilterPolicy fp = new SNSMessageFilterPolicy();

            // Add a filter policy attribute with a single value
            fp.addAttribute("store", "example_corp");
            fp.addAttribute("event", "order_placed");

            // Add a prefix attribute
            fp.addAttributePrefix("customer_interests", "bas");

            // Add an anything-but attribute
            fp.addAttributeAnythingBut("customer_interests", "baseball");

            // Add a filter policy attribute with a list of values
            ArrayList<String> attributeValues = new ArrayList<>();
            attributeValues.add("rugby");
            attributeValues.add("soccer");
            attributeValues.add("hockey");
            fp.addAttribute("customer_interests", attributeValues);

            // Add a numeric attribute
            fp.addAttribute("price_usd", "=", 0);

            // Add a numeric attribute with a range
            fp.addAttributeRange("price_usd", ">", 0, "<=", 100);

            // Apply the filter policy attributes to an Amazon SNS subscription
            fp.apply(snsClient, subscriptionArn);

        } catch (SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
+  For API details, see [SetSubscriptionAttributes](https://docs.aws.amazon.com/goto/SdkForJavaV2/sns-2010-03-31/SetSubscriptionAttributes) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
  

```
class SnsWrapper:
    """Encapsulates Amazon SNS topic and subscription functions."""
    def __init__(self, sns_resource):
        """
        :param sns_resource: A Boto3 Amazon SNS resource.
        """
        self.sns_resource = sns_resource

    def add_subscription_filter(subscription, attributes):
        """
        Adds a filter policy to a subscription. A filter policy is a key and a
        list of values that are allowed. When a message is published, it must have an
        attribute that passes the filter or it will not be sent to the subscription.

        :param subscription: The subscription the filter policy is attached to.
        :param attributes: A dictionary of key-value pairs that define the filter.
        """
        try:
            att_policy = {key: [value] for key, value in attributes.items()}
            subscription.set_attributes(
                AttributeName='FilterPolicy', AttributeValue=json.dumps(att_policy))
            logger.info("Added filter to subscription %s.", subscription.arn)
        except ClientError:
            logger.exception(
                "Couldn't add filter to subscription %s.", subscription.arn)
            raise
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/sns#code-examples)\. 
+  For API details, see [SetSubscriptionAttributes](https://docs.aws.amazon.com/goto/boto3/sns-2010-03-31/SetSubscriptionAttributes) in *AWS SDK for Python \(Boto3\) API Reference*\. 

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