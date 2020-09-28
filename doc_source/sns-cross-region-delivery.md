# Cross\-region delivery<a name="sns-cross-region-delivery"></a>

Amazon SNS supports cross\-region deliveries, both for Regions that are enabled by default and for [opt\-in Regions](#opt-in-regions)\. For the current list of AWS Regions that Amazon SNS supports, including opt\-in Regions, see [Amazon Simple Notification Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html) in the *Amazon Web Services General Reference\.* 

Amazon SNS supports the cross\-region delivery of notifications to Amazon SQS queues and to AWS Lambda functions\. When one of the Regions is an opt\-in Region, you must specify a different Amazon SNS service principal in the subscribed resource's policy\.

## Opt\-in Regions<a name="opt-in-regions"></a>

Amazon SNS supports the following opt\-in Regions: 
+ Africa \(Cape Town\)
+ Asia Pacific \(Hong Kong\)
+ Europe \(Milan\)
+ Middle East \(Bahrain\)

For information on enabling an opt\-in Region, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *Amazon Web Services General Reference\.*

When you use Amazon SNS to deliver messages from opt\-in Regions to Regions that are enabled by default, you must alter the resource policy created for the queue\. Replace the principal `sns.amazonaws.com` with `sns.<opt-in-region>.amazonaws.com`\. For example:
+  To subscribe an Amazon SQS queue in US East \(N\. Virginia\) to an SNS topic in Asia Pacific \(Hong Kong\), change the principal in the queue policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Middle East \(Bahrain\), EU \(Milano\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\. 
**Note**  
AWS also supports cross\-region delivery to Amazon SQS from a region that is enabled by default to an opt\-in region\. However, cross\-region forwarding of SNS messages from opt\-in regions to other opt\-in regions is not supported\. 
+  To subscribe an AWS Lambda function in US East \(N\. Virginia\) to an SNS topic in Asia Pacific \(Hong Kong\), change the principal in the AWS Lambda function policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Middle East \(Bahrain\), EU \(Milano\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\. 
**Note**  
AWS doesn't support cross\-region delivery to AWS Lambda from a region that is enabled by default to an opt\-in region\. Also, cross\-region forwarding of SNS messages from opt\-in regions to other opt\-in regions is not supported\. 