# Tutorial: To deploy and subscribe the event replay pipeline<a name="deploy-event-replay-pipeline"></a>

This tutorial shows how to deploy the [Event Replay Pipeline](sns-fork-pipeline-as-subscriber.md#sns-fork-event-replay-pipeline) and subscribe it to an Amazon SNS topic\. This process automatically turns the AWS SAM template associated with the pipeline into an AWS CloudFormation stack, and then deploys the stack into your AWS account\. This process also creates and configures the set of resources that comprise the Event Replay Pipeline, including an Amazon SQS queue and a Lambda function\.

For more information about filtering events, see [Amazon SNS subscription filter policies](sns-subscription-filter-policies.md) in this guide\.

1. Sign in to the [AWS Lambda console](https://console.aws.amazon.com/lambda/)\.

1. On the navigation panel, choose **Functions** and then choose **Create function**\.

1. On the **Create function** page, do the following:

   1. Choose **Browse serverless app repository**, **Public applications**, **Show apps that create custom IAM roles or resource policies**\.

   1. Search for `fork-event-replay-pipeline` and then choose the application\.

1. On the **fork\-event\-replay\-pipeline** page, do the following:

   1. In the **Application settings** section, enter an **Application name** \(for example, `my-app-replay`\)\.
**Note**  
For each deployment, the application name must be unique\. If you reuse an application name, the deployment will update only the previously deployed AWS CloudFormation stack \(rather than create a new one\)\.

   1. \(Optional\) Enter one of the following **LogLevel** settings for the execution of your application's Lambda function:
      + `DEBUG`
      + `ERROR`
      + `INFO` \(default\)
      + `WARNING`

   1. \(Optional\) For **ReplayQueueRetentionPeriodInSeconds**, enter the amount of time, in seconds, for which the Amazon SQS replay queue keeps the message\. If you don't enter a value, 1,209,600 seconds \(14 days\) is used\.

   1. For **TopicArn**, enter the ARN of the Amazon SNS topic to which this instance of the fork pipeline is to be subscribed\.

   1. For **DestinationQueueName**, enter the name of the Amazon SQS queue to which the Lambda replay function forwards messages\.

   1. \(Optional\) For **SubscriptionFilterPolicy**, enter the Amazon SNS subscription filter policy, in JSON format, to be used for filtering incoming events\. The filter policy decides which events are buffered for replay\. If you don't enter a value, no filtering is used \(all events are buffered for replay\)\.

   1. Choose **I acknowledge that this app creates custom IAM roles, resource policies and deploys nested applications\.** and then choose **Deploy**\.

On the **Deployment status for *my\-app\-replay*** page, Lambda displays the **Your application is being deployed** status\.

In the **Resources** section, AWS CloudFormation begins to create the stack and displays the **CREATE\_IN\_PROGRESS** status for each resource\. When the process is complete, AWS CloudFormation displays the **CREATE\_COMPLETE** status\.

When the deployment is complete, Lambda displays the **Your application has been deployed** status\.

Messages published to your Amazon SNS topic are buffered for replay in the Amazon SQS queue provisioned by the Event Replay Pipeline automatically\.

**Note**  
By default, replay is disabled\. To enable replay, navigate to the function's page on the Lambda console, expand the **Designer** section, choose the **SQS** tile and then, in the **SQS** section, choose **Enabled**\.