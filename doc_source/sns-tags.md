# Amazon SNS tags<a name="sns-tags"></a>

To organize and identify your Amazon SNS resources \(for example, for cost allocation or for organizing and discovering tagged topics\), you can add metadata *tags* that identify a topic's purpose, owner, or environment—this is especially useful when you have many topics\. For information about managing Amazon SNS topics using the AWS Management Console or the AWS SDK for Java \(and the `[TagResource](https://docs.aws.amazon.com/sns/latest/api/API_TagResource.html)`, `[UntagResource](https://docs.aws.amazon.com/sns/latest/api/API_UntagResource.html)`, and `[ListTagsForResource](https://docs.aws.amazon.com/sns/latest/api/API_ListTagsForResource.html)` API actions\), see the [Tutorial: Listing, adding, and removing tags for an Amazon SNS topic](sns-tutorial-list-add-remove-tags-for-topic.md) tutorial\.

**Note**  
Currently, tag\-based access control isn't available\.

You can use cost allocation tags to organize your AWS bill to reflect your own cost structure\. To do this, sign up to get your AWS account bill to include tag keys and values\. For more information, see [Setting Up a Monthly Cost Allocation Report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html#allocation-report) in the *AWS Billing and Cost Management User Guide*\.

Each tag consists of a key\-value pair that you define\. For example, you can easily identify your *production* and *testing* topics if you tag your topics as follows:


****  

| Topic | Key | Value | 
| --- | --- | --- | 
| MyTopicA | TopicType | Production | 
| MyTopicB | TopicType | Testing | 

**Note**  
When you use tags, keep the following guidelines in mind:  
We don't recommend adding more than 50 tags to a topic\.
Tags don't have any semantic meaning\. Amazon SNS interprets tags as character strings\.
Tags are case\-sensitive\.
A new tag with a key identical to that of an existing tag overwrites the existing tag\.
Tagging actions are limited to 10 TPS per AWS account, per AWS region\. If your application requires a higher throughput, [submit a request](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-sns)\.