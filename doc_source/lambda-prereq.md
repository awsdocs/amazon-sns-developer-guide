# Prerequisites<a name="lambda-prereq"></a>

To invoke Lambda functions using Amazon SNS notifications, you need the following:
+ Lambda function
+ Amazon SNS topic

For information about creating a Lambda function to use with Amazon SNS, see [Using Lambda with Amazon SNS](https://docs.aws.amazon.com/lambda/latest/dg/with-sns-example.html)\. For information about creating an Amazon SNS topic, see [Create a topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html)\.

 When you use Amazon SNS to deliver messages from opt\-in regions to regions which are enabled by default, you must alter the policy created in the AWS Lambda function by replacing the principal `sns.amazonaws.com` with `sns.<opt-in-region>.amazonaws.com`\. 

 For example, if you want to subscribe an AWS Lambda function in US East \(N\. Virginia\) to an SNS topic in Asia Pacific \(Hong Kong\), change the principal in the AWS Lambda function policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Middle East \(Bahrain\), EU \(Milano\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\. 

**Note**  
We do not support cross\-region delivery to AWS Lambda from a region that is enabled by default to an opt\-in region\. Also, cross\-region forwarding of SNS messages from opt\-in regions to other opt\-in regions is not supported\. 