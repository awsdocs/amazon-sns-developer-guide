# Subscribing AWS Event Fork Pipelines to an Amazon SNS topic<a name="sns-tutorial-subscribe-event-fork-pipelines"></a>

To accelerate the development of your event\-driven applications, you can subscribe event\-handling pipelines—powered by AWS Event Fork Pipelines—to Amazon SNS topics\. AWS Event Fork Pipelines is a suite of open\-source [nested applications](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-nested-applications.html), based on the [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/) \(AWS SAM\), which you can deploy directly from the [AWS Event Fork Pipelines suite](https://serverlessrepo.aws.amazon.com/applications?query=aws-event-fork-pipelines) \(choose **Show apps that create custom IAM roles or resource policies**\) into your AWS account\. For more information, see [How AWS Event Fork Pipelines works](sns-fork-pipeline-as-subscriber.md#how-sns-fork-works)\.

The following tutorials show how you can use the AWS Management Console to deploy a pipeline and then subscribe AWS Event Fork Pipelines to an Amazon SNS topic\. Before you begin, [create an Amazon SNS topic](sns-tutorial-create-topic.md)\.

To delete the resources that comprise a pipeline, find the pipeline on the **Applications** page of on the AWS Lambda console, expand the **SAM template section**, choose **CloudFormation stack**, and then choose **Other Actions**, **Delete Stack**\.

**Topics**
+ [Tutorial: To deploy and subscribe the event storage and backup pipeline](deploy-event-storage-backup-pipeline.md)
+ [Tutorial: To deploy and subscribe the event search and analytics pipeline](deploy-event-search-analytics-pipeline.md)
+ [Tutorial: To deploy and subscribe the event replay pipeline](deploy-event-replay-pipeline.md)