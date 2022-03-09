# Amazon SNS topic tagging<a name="sns-tags"></a>

Amazon SNS supports tagging of Amazon SNS topics\. This can help you track and manage the costs associated with your topics, provide enhanced security in your AWS Identity and Access Management \(IAM\) policies, and lets you easily search or filter through thousands of topics\. Tagging enables you to manage your Amazon SNS topics using AWS Resource Groups\. For more information on Resource Groups, see the [AWS Resource Groups User Guide](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html)\.

**Topics**
+ [Tagging for cost allocation](#tagging-for-cost-allocation)
+ [Tagging for access control](#sns-tagging-for-access-control)
+ [Tagging for resource searching and filtering](#sns-tagging-for-searching-filtering)
+ [Configuring tags](sns-tags-configuring.md)

## Tagging for cost allocation<a name="tagging-for-cost-allocation"></a>

To organize and identify your Amazon SNS topics for cost allocation, you can add tags that identify the purpose of a topic\. This is especially useful when you have many topics\. You can use cost allocation tags to organize your AWS bill to reflect your own cost structure\. To do this, sign up to get your AWS account bill to include the tag keys and values\. For more information, see [Setting Up a Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html#allocation-report) in the [AWS Billing and Cost Management User Guide](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)\.

For example, you can add tags that represent the cost center and purpose of your Amazon SNS topics, as follows:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-tags.html)

This tagging scheme lets you to group two topics performing related tasks in the same cost center, while tagging an unrelated activity with a different cost allocation tag\.

## Tagging for access control<a name="sns-tagging-for-access-control"></a>

AWS Identity and Access Management supports controlling access to resources based on tags\. After tagging your resources, provide information about your resource tags in the condition element of an IAM policy to manage tag\-based access\. For information on how to tag your resources using the [Amazon SNS console](sns-tags-configuring.md#list-add-update-remove-tags-for-topic-aws-console) or the [AWS SDK](sns-tags-configuring.md#tag-resource-aws-sdks), see [Configuring tags](sns-tags-configuring.md)\.

You can restrict access for an IAM identity\. For example, you can restrict `Publish` and `PublishBatch` access to all Amazon SNS topics that include a tag with the key `environment` and the value `production`, while allowing access to all other Amazon SNS topics\. In the example below, the policy restricts the ability to publish messages to topics tagged with `production`, while allowing messages to be published to topics tagged with `development`\. For more information, see [Controlling Access Using Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html) in the IAM User Guide\.

**Note**  
Setting the IAM permission for `Publish` sets permission for both `Publish` and `PublishBatch`\.

```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": [
	"sns:Publish"
    ],
    "Resource": "arn:aws:sns:*:*:*",
    "Condition": {
      "StringEquals": {
        "aws:ResourceTag/environment": "production"
      }
    }
  },
  {
    "Effect": "Allow",
    "Action": [
      "sns:Publish"
    ],
    "Resource": "arn:aws:sns:*:*:*",
    "Condition": {
      "StringEquals": {
        "aws:ResourceTag/environment": "development"
      }
    }
  }]
}
```

## Tagging for resource searching and filtering<a name="sns-tagging-for-searching-filtering"></a>

An AWS account can have tens of thousands of Amazon SNS topics \(see [Amazon SNS Quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html) for details\)\. By tagging your topics, you can simplify the process of searching through or filtering out topics\.

For example, you may have hundreds of topics associated with your production environment\. Rather than having to manually search for these topics, you can query for all topics with a given tag:

```
import com.amazonaws.services.resourcegroups.AWSResourceGroups;
import com.amazonaws.services.resourcegroups.AWSResourceGroupsClientBuilder;
import com.amazonaws.services.resourcegroups.model.QueryType;
import com.amazonaws.services.resourcegroups.model.ResourceQuery;
import com.amazonaws.services.resourcegroups.model.SearchResourcesRequest;
import com.amazonaws.services.resourcegroups.model.SearchResourcesResult;

public class Example {
    public static void main(String[] args) {
        // Query Amazon SNS Topics with tag "keyA" as "valueA"
        final String QUERY = "{\"ResourceTypeFilters\":[\"AWS::SNS::Topic\"],\"TagFilters\":[{\"Key\":\"keyA\", \"Values\":[\"valueA\"]}]}";

        // Initialize ResourceGroup client
        AWSResourceGroups awsResourceGroups = AWSResourceGroupsClientBuilder
            .standard()
            .build();

        // Query all resources with certain tags from ResourceGroups 
        SearchResourcesResult result = awsResourceGroups.searchResources(
            new SearchResourcesRequest().withResourceQuery(
                new ResourceQuery()
                .withType(QueryType.TAG_FILTERS_1_0)
                .withQuery(QUERY)
            ));
        System.out.println("SNS Topics with certain tags are " + result.getResourceIdentifiers());
    }
}
```