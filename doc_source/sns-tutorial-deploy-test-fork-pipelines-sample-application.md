# Tutorial: Deploying and Testing the AWS Event Fork Pipelines Sample Application<a name="sns-tutorial-deploy-test-fork-pipelines-sample-application"></a>

To accelerate the development of your event\-driven applications, you can subscribe event\-handling pipelines—powered by AWS Event Fork Pipelines—to Amazon SNS topics\. AWS Event Fork Pipelines is a suite of open\-source [nested applications](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-nested-applications.html), based on the [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/) \(AWS SAM\), which you can deploy directly from the [AWS Event Fork Pipelines suite](https://serverlessrepo.aws.amazon.com/applications?query=aws-event-fork-pipelines) \(choose **Show apps that create custom IAM roles or resource policies**\) into your AWS account\. For more information, see [How AWS Event Fork Pipelines Works](sns-fork-pipeline-as-subscriber.md#how-sns-fork-works)\.

The following tutorial shows how you can use the AWS Management Console to deploy and test the AWS Event Fork Pipelines sample application\.

**Important**  
To avoid incurring unwanted costs after you finish deploying the AWS Event Fork Pipelines sample application, delete its AWS CloudFormation stack\. For more information, see [Deleting a Stack on the AWS CloudFormation Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-delete-stack.html) in the *AWS CloudFormation User Guide*\.

**Topics**
+ [Example AWS Event Fork Pipelines Use Case](example-sns-fork-use-case.md)
+ [To Deploy the Sample Application](deploy-sample-application.md)
+ [To Execute the Sample Application](execute-sample-application.md)
+ [To Verify the Execution of the Sample Application and Its Pipelines](verify-sample-application-pipelines.md)
+ [To Simulate an Issue and Replay Events for Recovery](simulate-issue-replay-events-for-recovery.md)