# Sending Amazon SNS messages to an Amazon SQS queue or AWS Lambda function in a different Region<a name="sns-cross-region-delivery"></a>

Amazon SNS supports cross\-region deliveries, both for Regions that are enabled by default and for [opt\-in Regions](#opt-in-regions)\. For the current list of AWS Regions that Amazon SNS supports, including opt\-in Regions, see [Amazon Simple Notification Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html) in the *Amazon Web Services General Reference\.* 

Amazon SNS supports the cross\-region delivery of notifications to Amazon SQS queues and to AWS Lambda functions\. When one of the Regions is an opt\-in Region, you must specify a different Amazon SNS service principal in the subscribed resource's policy\.

## Opt\-in Regions<a name="opt-in-regions"></a>

Amazon SNS supports the following opt\-in Regions: 
+ Africa \(Cape Town\)
+ Asia Pacific \(Hong Kong\)
+ Asia Pacific \(Jakarta\)
+ Europe \(Milan\)
+ Middle East \(Bahrain\)

For information on enabling an opt\-in Region, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *Amazon Web Services General Reference\.*

When you use Amazon SNS to deliver messages from opt\-in Regions to Regions that are enabled by default, you must alter the resource policy created for the queue\. Replace the principal `sns.amazonaws.com` with `sns.<opt-in-region>.amazonaws.com`\. For example:
+  To subscribe an Amazon SQS queue in US East \(N\. Virginia\) to an Amazon SNS topic in Asia Pacific \(Hong Kong\), change the principal in the queue policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Asia Pacific \(Jakarta\), Middle East \(Bahrain\), Europe \(Milan\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\.   
**Cross\-region delivery support to Amazon SQS**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)

  The following is an example of an access policy statement that allows an Amazon SNS topic in an opt\-in Region \(CPT Region\) to deliver to an Amazon SQS queue in an enabled\-by\-default Region \(IAD Region\)\. It contains the necessary regionalized service principal configuration under the path `Statement`/`Principal`/`Service`\. 

  ```
  {
    "Version": "2008-10-17",
    "Id": "__default_policy_ID",
    "Statement": [
      {
        "Sid": "allow_sns_arn:aws:sns:af-south-1:111111111111:source_topic_name",
        "Effect": "Allow",
        "Principal": {
          "Service": "sns.af-south-1.amazonaws.com"
        },
        "Action": "SQS:SendMessage",
        "Resource": "arn:aws:sqs:us-east-1:111111111111:destination_queue_name",
        "Condition": {
          "ArnLike": {
            "aws:SourceArn": "arn:aws:sns:af-south-1:111111111111:source_topic_name"
          }
        }
      },
      ...
    ]
  }
  ```
+  To subscribe an AWS Lambda function in US East \(N\. Virginia\) to an Amazon SNS topic in Asia Pacific \(Hong Kong\), change the principal in the AWS Lambda function policy to `sns.ap-east-1.amazonaws.com`\. Opt\-in regions include any regions launched after March 20, 2019, which includes Asia Pacific \(Hong Kong\), Asia Pacific \(Jakarta\), Middle East \(Bahrain\), Europe \(Milan\), and Africa \(Cape Town\)\. Regions launched prior to March 20, 2019 are enabled by default\.   
**Cross\-region delivery support to AWS Lambda**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-cross-region-delivery.html)