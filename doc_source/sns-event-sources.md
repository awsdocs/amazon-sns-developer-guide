# Amazon SNS event sources<a name="sns-event-sources"></a>

This page lists the AWS services that can publish events to Amazon SNS topics, grouped by their [AWS product categories](https://aws.amazon.com/products/)\.

**Note**  
Amazon SNS introduced [FIFO topics](sns-fifo-topics.md) in October, 2020\. Currently, most AWS services support sending events to standard topics only\.

## Analytics services<a name="sns-event-sources-analytics"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html) – Allows you to analyze data in Amazon S3 using standard SQL\.  |  Receive notifications when control limits are exceeded\. For more information, see [Setting data usage control limits](https://docs.aws.amazon.com/athena/latest/ug/workgroups-setting-control-limits-cloudwatch.html) in the *Amazon Athena User Guide*\.  | 
|  [AWS Data Pipeline](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/what-is-datapipeline.html) – Helps automate the movement and transformation of data\.  |  Receive notifications about the status of pipeline components\. For more information, see [SnsAlarm](https://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-object-snsalarm.html) in the *AWS Data Pipeline Developer Guide*\.  | 
|  [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html) – Manages all of the work of setting up, operating, and scaling a data warehouse\.  |  Receive notifications of Amazon Redshift events\. For more information, see [Amazon Redshift event notifications](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html) in the *Amazon Redshift Cluster Management Guide*\.  | 

## Application integration services<a name="sns-event-sources-application-integration"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html) – Delivers a stream of real\-time data from your own applications, software\-as\-a\-service \(SaaS\) applications, and AWS services and routes that data to targets, including Amazon SNS\. EventBridge was formerly called CloudWatch Events\.  |  Receive notifications of EventBridge events\. For more information, see [Amazon EventBridge targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-targets.html) in the *Amazon EventBridge User Guide*\.  | 
|  [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html) – Lets you combine AWS Lambda functions and other AWS services to build business\-critical applications\.  |  Receive notification of Step Functions events\. For more information, see [Call Amazon SNS with Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/connect-sns.html) in the *AWS Step Functions Developer Guide*\.  | 

## Billing & cost management services<a name="sns-event-sources-billing-cost-management"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Billing and Cost Management](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html) – Provides features that help you monitor your costs and pay your bill\.  |  Receive budget notifications, price change notifications, and anomaly alerts\. For more information, see the following pages in the AWS Billing User Guide: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-event-sources.html)  | 

## Business applications services<a name="sns-event-sources-business-applications"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Chime](https://docs.aws.amazon.com/chime/latest/dg/what-is-chime.html) – Lets you meet, chat, and place business calls inside and outside of your organization\.  |  Receive important meeting event notifications\. For more information, see [Amazon Chime SDK event notifications](https://docs.aws.amazon.com/chime/latest/dg/mtgs-sdk-notifications.html) in the *Amazon Chime Developer Guide*\.  | 

## Compute services<a name="sns-event-sources-compute"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html) – Helps you have the correct number of Amazon Elastic Compute Cloud \(Amazon EC2\) instances available for handling your application's load\.  |  Receive notifications when Auto Scaling launches or terminates Amazon EC2 instances in your Auto Scaling group\. For more information, see [Getting Amazon SNS notifications when your Auto Scaling group scales](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ASGettingNotifications.html) in the *Amazon EC2 Auto Scaling User Guide*\.  | 
|  [EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/what-is-image-builder.html) – Helps automate the creation, management, and deployment of customized, secure, and up\-to\-date server images that are pre\-installed and pre\-configured with software and settings to meet specific IT standards\.  |  Receive notifications when builds are complete\. For more information, see [Tracking the latest server images in EC2 Image Builder pipelines](https://aws.amazon.com/blogs/compute/tracking-the-latest-server-images-in-amazon-ec2-image-builder-pipelines/) on the *AWS Compute Blog*\.  | 
|  [AWS Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/welcome.html) – Handles the details of capacity provisioning, load balancing, and scaling for your application, and provides application health monitoring\.  |  Receive notifications of important events that affect your application\. For more information, see [Elastic Beanstalk environment notifications with Amazon SNS](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.sns.html) in the *AWS Elastic Beanstalk Developer Guide*\.  | 
|  [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) – Lets you run code without provisioning or managing servers\.  |  Receive function output data by setting an SNS topic as a Lambda dead\-letter queue or a Lambda destination\. For more information, see [Asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html) in the *AWS Lambda Developer Guide*\.  | 
|  [Amazon Lightsail](https://lightsail.aws.amazon.com/ls/docs/all) – Helps developers get started using AWS to build websites or web applications\.  |  Receive notifications when a metric for one of your instances, databases, or load balancers crosses a specified threshold\. For more information, see [Adding notification contacts in Amazon Lightsail](https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-adding-editing-notification-contacts) in the *Amazon Lightsail Developer Guide*\.  | 

## Containers services<a name="sns-event-sources-containers"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon EKS Distro](https://docs.aws.amazon.com/eks/latest/userguide/eks-distro.html) – Lets you create reliable and secure clusters wherever your applications are deployed\.  |  Track updates and security patches for clusters created with Amazon EKS Distro\. For more information, see [Introducing Amazon EKS Distro \- an open source Kubernetes distribution used by Amazon EKS](https://aws.amazon.com/about-aws/whats-new/2020/12/introducing-amazon-eks-distro/)\.  | 
|  [Amazon Elastic Container Service \(Amazon ECS\)](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) – Enables you to run, stop, and manage containers on a cluster\.  |  Receive notifications when a new Amazon ECS\-optimized AMI is available\. For more information, see [Subscribing to Amazon ECS\-optimized AMI update notifications](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS-AMI-SubscribeTopic.html) in the *Amazon Elastic Container Service Developer Guide*\.  | 

## Customer engagement services<a name="sns-event-sources-customer-engagement"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/what-is-amazon-connect.html) – Lets you set up an omnichannel cloud contact center to engage with your customers\.  |  Receive alerts and validations\. For more information, see [The power of AWS with Amazon Connect](https://docs.aws.amazon.com/connect/latest/adminguide/related-services-amazon-connect.html) in the *Amazon Connect Administrator Guide*\.  | 
|  [Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/welcome.html) – Helps you engage your customers by sending them email, SMS and voice messages, and push notifications\.  |  Configure two\-way SMS, which allows you to receive messages from your customers\. For more information, see [Using two\-way SMS messaging in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-two-way.html) in the *Amazon Pinpoint User Guide*\.  | 
|  [Amazon Simple Email Service \(Amazon SES\)](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/Welcome.html) – Provides cost\-effective way for you to send and receive email using your own email addresses and domains\.  |  Receive notifications of bounces, complaints, and deliveries\. For more information, see [Configuring Amazon SNS notifications for Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/configure-sns-notifications.html) in the *Amazon Simple Email Service Developer Guide*\.  | 

## Database services<a name="sns-event-sources-database"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/Introduction.html) – Migrates data from on\-premises databases into the AWS Cloud\.  |  Receive notifications when AWS DMS events occur; for example, when a replication instance is created or deleted\. For more information, see [Working with events and notifications in AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Events.html) in the *AWS Database Migration Service User Guide*\.  | 
|  [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) – Provides fast and predictable performance with seamless scalability in this fully managed NoSQL database service\.  |  Receive notifications when maintenance events occur\. For more information, see [Customizing DAX cluster settings](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.cluster-management.html#DAX.cluster-management.custom-settings) in the *Amazon DynamoDB Developer Guide*\. | 
|  [Amazon ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/WhatIs.html) – Provides a high performance, resizeable, and cost\-effective in\-memory cache, while removing complexity associated with deploying and managing a distributed cache environment\.  |  Receive notifications when significant events occur\. For more information, see [Event notifications and Amazon SNS](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ElastiCacheSNS.html) in the *Amazon ElastiCache for Memcached User Guide*\.  | 
|  [Amazon Neptune](https://docs.aws.amazon.com/neptune/latest/userguide/intro.html) – Enables you to build and run applications that work with highly connected datasets\.  |  Receive notifications when a Neptune event occurs\. For more information, see [Using Neptune event notification](https://docs.aws.amazon.com/neptune/latest/userguide/events.html) in the *Neptune User Guide*\.  | 
|  [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/welcome.html) – Manages all of the work of setting up, operating, and scaling a data warehouse\.  |  Receive notifications of Amazon Redshift events\. For more information, see [Amazon Redshift event notifications](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html) in the *Amazon Redshift Cluster Management Guide*\.  | 
|  [Amazon Relational Database Service](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Welcome.html) – Makes it easier to set up, operate, and scale a relational database in the AWS Cloud\.  |  Receive notifications of Amazon RDS events\. For more information, see [Using Amazon RDS event notification](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Events.html) in the *Amazon RDS User Guide*\.  | 

## Developer tools services<a name="sns-event-sources-developer-tools"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html) – Compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\.  |  Receive notifications when builds succeed, fail, or move from one build phase to another\. For more information, see [Build notifications sample for CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-build-notifications.html) in the *AWS CodeBuild User Guide*\.  | 
|  [AWS CodeCommit](https://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) – Provides version control for privately storing and managing assets in the cloud\.  |  Receive notifications about CodeCommit repository events\. For more information, see [Example: Create an AWS CodeCommit trigger for an Amazon SNS topic](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-sns.html) in the *AWS CodeCommit User Guide*\.  | 
|  [AWS CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html) – Automates application deployments to Amazon EC2 instances, on\-premises instances, serverless Lambda functions, or Amazon ECS services\.  |  Receive notifications for CodeDeploy deployments or instance events\. For more information, see [Create a trigger for a CodeDeploy event](https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-sns-event-notifications-create-trigger.html) in the *AWS CodeDeploy User Guide*\.  | 
|  [Amazon CodeGuru](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/what-is-codeguru-profiler.html) – Collects runtime performance data from your live applications, and provides recommendations that can help you fine\-tune your application performance\.  |  Receive notifications when anomalies occur\. For more information, see [Working with anomalies and recommendation reports](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/working-with-recommendation-reports.html) in the *Amazon CodeGuru User Guide*\.  | 
|  [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html) – Automates the steps required to release software changes continuously\.  |  Receive notifications about approval actions\. For more information, see [Manage approval actions in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals.html) in the *AWS CodePipeline User Guide*\.  | 
|  [AWS CodeStar](https://docs.aws.amazon.com/codestar/latest/userguide/welcome.html) – Create, manage, and work with software development projects on AWS\.  |  Receive notifications about events that occur in the resources that you use\. For more information, see [Configure Amazon SNS topics for notifications](https://docs.aws.amazon.com/dtconsole/latest/userguide/set-up-sns.html) in the *Developer Tools Console User Guide*\.  | 

## Front\-end web & mobile services<a name="sns-event-sources-front-end-web-mobile"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/welcome.html) – Helps you engage your customers by sending them email, SMS and voice messages, and push notifications\.  |  Configure two\-way SMS, which allows you to receive messages from your customers\. For more information, see [Using two\-way SMS messaging in Amazon Pinpoint](https://docs.aws.amazon.com/pinpoint/latest/userguide/channels-sms-two-way.html) in the *Amazon Pinpoint User Guide*\.  | 

## Game development services<a name="sns-event-sources-game-development"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon GameLift](https://docs.aws.amazon.com/gamelift/latest/developerguide/gamelift-intro.html) – Provides solutions for hosting session\-based multiplayer game servers in the cloud, including a fully managed service for deploying, operating, and scaling game servers\.  |  Receive matchmaking and queue event notifications\. For more information, see the following pages: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-event-sources.html)  | 

## Internet of Things services<a name="sns-event-sources-iot"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS IoT Core](https://docs.aws.amazon.com/iot/latest/developerguide/what-is-aws-iot.html) – Provides the cloud services that connect your IoT devices to other devices and AWS Cloud services\.  |  Receive notifications of AWS IoT Core events\. For more information, see [Creating an Amazon SNS rule](https://docs.aws.amazon.com/iot/latest/developerguide/iot-sns-rule.html) in the *AWS IoT Developer Guide*\.  | 
|  [AWS IoT Device Defender](https://docs.aws.amazon.com/iot/latest/developerguide/device-defender.html) – Allows you to audit the configuration of your devices, monitor connected devices to detect abnormal behavior, and mitigate security risks\.  |  Receive alarms when a device violates a behavior\. For more information, see [How to use AWS IoT Device Defender detect](https://docs.aws.amazon.com/iot/latest/developerguide/detect-HowToHowTo.html) in the *AWS IoT Developer Guide*\.  | 
|  [AWS IoT Events](https://docs.aws.amazon.com/iotevents/latest/developerguide/what-is-iotevents.html) – Lets you monitor your equipment or device fleets for failures or changes in operation, and trigger actions when such events occur\.  |  Receive notifications of AWS IoT Events events\. For more information, see [Amazon Simple Notification Service](https://docs.aws.amazon.com/iotevents/latest/developerguide/iotevents-other-aws-services.html#iotevents-sns) in the *AWS IoT Events Developer Guide*\.  | 
|  [AWS IoT Greengrass](https://docs.aws.amazon.com/greengrass/latest/developerguide/what-is-iot-greengrass.html) – Extends AWS onto physical devices so they can act locally on the data they generate, while still using the cloud for management, analytics, and durable storage\.  |  Receive notifications of AWS IoT Greengrass events\. For more information, see [SNS connector](https://docs.aws.amazon.com/greengrass/latest/developerguide/sns-connector.html) in the *AWS IoT Greengrass Version 1 Developer Guide*\.  | 

## Machine learning services<a name="sns-event-sources-machine-learning"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon CodeGuru](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/what-is-codeguru-profiler.html) – Collects runtime performance data from your live applications, and provides recommendations that can help you fine\-tune your application performance\.  |  Receive notifications when anomalies occur\. For more information, see [Working with anomalies and recommendation reports](https://docs.aws.amazon.com/codeguru/latest/profiler-ug/working-with-recommendation-reports.html) in the *Amazon CodeGuru User Guide*\.  | 
|  [Amazon DevOps Guru](https://docs.aws.amazon.com/devops-guru/latest/userguide/welcome.html) – Generates operational insights using machine learning to help you improve the performance of your operational applications\.  |  Forward insights and confirmations\. For more information, see [Deliver ML\-powered operational insights to your on\-call teams via PagerDuty with Amazon DevOps Guru](https://aws.amazon.com/blogs/mt/deliver-ml-powered-operational-insights-to-your-on-call-teams-via-pagerduty-with-amazon-devops-guru/) on the *AWS Management & Governance Blog*\.  | 
|  [Amazon Lookout for Metrics](https://docs.aws.amazon.com/lookoutmetrics/latest/dev/lookoutmetrics-welcome.html) – Finds anomalies in your data, determines their root causes, and enables you to quickly take action\.  |  Receive notifications of anomalies\. For more information, see [Using Amazon SNS with Lookout for Metrics](https://docs.aws.amazon.com/lookoutmetrics/latest/dev/services-sns.html) in the *Amazon Lookout for Metrics Developer Guide*\.  | 
|  [Amazon Rekognition](https://docs.aws.amazon.com/rekognition/latest/dg/what-is.html) – Lets you add image and video analysis to your applications  |  Receive notifications of request results\. For more information, see [Reference: Video analysis results notification](https://docs.aws.amazon.com/rekognition/latest/dg/video-notification-payload.html) in the *Amazon Rekognition Developer Guide*\.  | 
|  [Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) – Enables data scientists and developers to build and train machine learning models, and then directly deploy them into a production\-ready hosted environment\.  |  Receive notifications when a data object is labeled\. For more information, see [Creating a streaming labeling job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-streaming-create-job.html) in the *Amazon SageMaker Developer Guide*\.  | 

## Management & governance services<a name="sns-event-sources-management-governance"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/what-is.html) – Enables DevOps and software development teams to use Amazon Chime and Slack chat rooms to monitor and respond to operational events in the AWS Cloud\.  |  Deliver notifications to chat rooms\. For more information, see [Setting up AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/setting-up.html) in the *AWS Chatbot Administrator Guide*\.  | 
|  [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) – Enables you to create and provision AWS infrastructure deployments predictably and repeatedly\.  |  Receive notifications when stacks are created and updated\. For more information, see [Setting AWS CloudFormation stack options](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-add-tags.html) in the *AWS CloudFormation User Guide*\.  | 
|  [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/WhatIsCloudWatch.html) – Provides event history of your AWS account activity\.  |  Receive notifications when CloudTrail publishes new log files to your Amazon S3 bucket\. For more information, see [Configuring Amazon SNS notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/configure-sns-notifications-for-cloudtrail.html) in the *AWS CloudTrail User Guide*\.  | 
|  [Amazon CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) – Monitors your AWS resources and the applications you run on AWS in real time\.  |  Receive notifications when alarms change state\. For more information, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.  | 
|  [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) – Provides a detailed view of the configuration of AWS resources in your AWS account\.  |  Receive notifications when resources are updated, or when AWS Config evaluates custom or managed rules against your resources\. For more information, see [Notifications that AWS Config sends to an SNS topic](https://docs.aws.amazon.com/config/latest/developerguide/notifications-for-AWS-Config.html) and [Example configuration item change notifications](https://docs.aws.amazon.com/config/latest/developerguide/example-sns-notification.html) in the *AWS Config Developer Guide*\.  | 
|  [AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html) – Enables you to set up and govern a secure, compliant, multi\-account AWS environment\.  |  Use alerts to help you prevent drift within your landing zone, and receive compliance notifications\. For more information, see [Tracking alerts through Amazon Simple Notification Service](https://docs.aws.amazon.com/controltower/latest/userguide/sns.html) in the *AWS Control Tower User Guide*\.  | 
|  [AWS License Manager](https://docs.aws.amazon.com/license-manager/latest/userguide/license-manager.html) – Helps you manage your software licenses from software vendors centrally across AWS and your on\-premises environments\.  |  Receive License Manager notifications and alerts\. For more information, see [Settings in License Manager](https://docs.aws.amazon.com/license-manager/latest/userguide/settings.html) in the *License Manager User Guide* and [Creating ServiceNow incidents for AWS License Manager notifications](https://aws.amazon.com/blogs/mt/servicenow-incidents-for-license-manager/) on the* AWS Management & Governance Blog*\.  | 
|  [AWS Service Catalog](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/introduction.html) – Enables IT administrators to create, manage, and distribute portfolios of approved products to end users, who can then access the products they need in a personalized portal\.   |  Receive notifications about stack events\. For more information, see [AWS Service Catalog notification constraints](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/constraints-notification.html) in the *AWS Service Catalog Administrator Guide*\.  | 
|  [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html) – Lets you view and control your infrastructure on AWS\.  |  Receive notifications about the status of commands\. For more information, see [Monitoring Systems Manager status changes using Amazon SNS notifications](https://docs.aws.amazon.com/systems-manager/latest/userguide/monitoring-sns-notifications.html) in the *AWS Systems Manager User Guide*\.  | 

## Media services<a name="sns-event-sources-media"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon Elastic Transcoder](https://docs.aws.amazon.com/elastictranscoder/latest/developerguide/introduction.html) – Lets you convert media files that you stored in Amazon S3 into media files in the formats required by consumer playback devices\.  |  Receive notifications when jobs change status\. For more information, see [Notifications of job status](https://docs.aws.amazon.com/elastictranscoder/latest/developerguide/notifications.html) in the *Amazon Elastic Transcoder Developer Guide*\.  | 

## Migration & transfer services<a name="sns-event-sources-migration-transfer"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Application Discovery Service](https://docs.aws.amazon.com/application-discovery/latest/userguide/what-is-appdiscovery.html) – Helps you plan your migration to the AWS Cloud by collecting usage and configuration data about your on\-premises servers\.  |  Receive notifications of events through AWS CloudTrail\. For more information, see [Logging Application Discovery Service API calls with AWS CloudTrail](https://docs.aws.amazon.com/application-discovery/latest/userguide/logging-using-cloudtrail.html) in the *Application Discovery Service User Guide*\.  | 
|  [AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/Introduction.html) – Migrates data from on\-premises databases into the AWS Cloud\.  |  Receive notifications when AWS DMS events occur; for example, when a replication instance is created or deleted\. For more information, see [Working with events and notifications in AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Events.html) in the *AWS Database Migration Service User Guide*\.  | 
|  [AWS Snowball](https://docs.aws.amazon.com/snowball/latest/ug/whatissnowball.html) – Uses physical storage devices to transfer large amounts of data between Amazon S3 and your onsite data storage location at faster\-than\-internet speeds\.  |  Receive notifications for Snowball jobs\. For more information, see the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-event-sources.html)  | 

## Networking & content delivery services<a name="sns-event-sources-networking-content-delivery"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html) – Enables you to create and deploy your own REST and WebSocket APIs at any scale\.  |  Receive messages posted to an API Gateway endpoint\. For more information, see [Tutorial: Build an API Gateway REST API with AWS integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-aws-proxy.html) in the *API Gateway Developer Guide*\.  | 
|  [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) – Speeds up distribution of your static and dynamic web content, such as \.html, \.css, \.php, image, and media files\.  |  Receive notifications when alarms based on specified CloudFront metrics occur\. For more information, see [Setting alarms to receive notifications](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/receiving-notifications.html) in the *Amazon CloudFront Developer Guide*\.  | 
|  [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) – Links your internal network to an AWS Direct Connect location over a standard Ethernet fiber\-optic cable\.  |  Receive notifications when alarms that monitor the state of an AWS Direct Connect connection change state\. For more information, see [Creating CloudWatch alarms to monitor AWS Direct Connect connections](https://docs.aws.amazon.com/directconnect/latest/UserGuide/monitoring-cloudwatch.html#creating-alarms) in the *AWS Direct Connect User Guide*\.  | 
|  [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/welcome.html) – Automatically distributes your incoming traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in more or more Availability Zones\.  |  Receive notifications of alarms you've created for load balancer events\. For more information, see [Create CloudWatch alarms for your load balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-cloudwatch-metrics.html#create_cw_alarms) in the *User Guide for Classic Load Balancers*\.  | 
|  [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) – Provides domain registration, DNS routing, and health checking\.  |  Receive notifications when health check status is unhealthy\. For more information, see [To receive an Amazon SNS notification when a health check status is unhealthy \(console\)](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/monitoring-health-checks.html#monitoring-sns-notification-procedure) in the *Amazon Route 53 Developer Guide*\.  | 
|  [Amazon Virtual Private Cloud \(Amazon VPC\)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) – Enables you to launch AWS resources into a virtual network that you've defined\.  |  Receive notifications for specific events that occur on interface endpoints\. For more information, see [Create and manage a notification for an endpoint service](https://docs.aws.amazon.com/vpc/latest/userguide/create-notification-endpoint-service.html) in the *Amazon VPC User Guide*\.  | 

## Security, identity, & compliance services<a name="sns-event-sources-security-identity-compliance"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Directory Service](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/what_is.html) – Provides multiple ways to use Microsoft Active Directory \(AD\) with other AWS services\.  |  Receive email or text \(SMS\) messages when the status of your directory changes\. For more information, see [Configure directory status notifications](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_enable_notifications.html) in the *AWS Directory Service Administration Guide*\.  | 
|  [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html) – Provides continuous security monitoring to help to identify unexpected and potentially unauthorized or malicious activity in your AWS environment\.  |  Receive notifications about newly released finding types, updates to the existing finding types, and other functionality changes\. For more information, see [Subscribing to GuardDuty announcements SNS topic](https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_sns.html) in the *Amazon GuardDuty User Guide*\.  | 
|  [Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_introduction.html) – Tests the network accessibility of your Amazon EC2 instances and the security state of your applications that run on those instances\.  |  Receive notifications for Amazon Inspector events\. For more information, see [Setting up an SNS topic for Amazon Inspector notifications](https://docs.aws.amazon.com/inspector/latest/userguide/inspector_assessments.html#sns-topic) in the *Amazon Inspector User Guide*\.  | 

## Serverless services<a name="sns-event-sources-serverless"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html) – Provides fast and predictable performance with seamless scalability in this fully managed NoSQL database service\.  |  Receive notifications when maintenance events occur\. For more information, see [Customizing DAX cluster settings](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.cluster-management.html#DAX.cluster-management.custom-settings) in the *Amazon DynamoDB Developer Guide*\. | 
|  [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html) – Delivers a stream of real\-time data from your own applications, software\-as\-a\-service \(SaaS\) applications, and AWS services and routes that data to targets, including Amazon SNS\. EventBridge was formerly called CloudWatch Events\.  |  Receive notifications of EventBridge events\. For more information, see [Amazon EventBridge targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-targets.html) in the *Amazon EventBridge User Guide*\.  | 
|  [AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) – Lets you run code without provisioning or managing servers\.  |  Receive function output data by setting an SNS topic as a Lambda dead\-letter queue or a Lambda destination\. For more information, see [Asynchronous invocation](https://docs.aws.amazon.com/lambda/latest/dg/invocation-async.html) in the *AWS Lambda Developer Guide*\.  | 

## Storage services<a name="sns-event-sources-storage"></a>


| AWS service | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) – Helps you centralize and automate the backup of data across AWS services in the cloud and on premises  |  Receive notifications of AWS Backup events\. For more information, see [Using Amazon SNS to track AWS Backup events](https://docs.aws.amazon.com/aws-backup/latest/devguide/sns-notifications.html) in the *AWS Backup Developer Guide*\.  | 
|  [Amazon Elastic File System](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html) – Provides file storage for your Amazon EC2 instances\.  |  Receive notifications of alarms you've created for Amazon EFS events\. For more information, see [Automated monitoring tools](https://docs.aws.amazon.com/efs/latest/ug/monitoring_automated_manual.html#monitoring_automated_tools) in the *Amazon Elastic File System User Guide*\.  | 
|  [Amazon S3 Glacier](https://docs.aws.amazon.com/amazonglacier/latest/dev/introduction.html) – Provides storage for infrequently used data\.  |  Set a notification configuration on a vault so that when a job completes, a message is sent to an SNS topic\. For more information, see [Configuring vault notifications in Amazon S3 Glacier](https://docs.aws.amazon.com/amazonglacier/latest/dev/configuring-notifications.html) in the *Amazon S3 Glacier Developer Guide*\.  | 
|  [Amazon Simple Storage Service \(Amazon S3\)](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/Welcome.html) – Provides object storage\.  |  Receive notifications when changes occur to an Amazon S3 bucket or in the rare instance when objects don't replicate to their destination Region\. For more information, see [Walkthrough: Configure a bucket for notifications \(SNS topic or SQS queue\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ways-to-add-notification-config-to-bucket.html) and [Monitoring progress with replication metrics and Amazon S3 event notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication-metrics.html) in the *Amazon Simple Storage Service User Guide*\.  | 
|  [AWS Snowball](https://docs.aws.amazon.com/snowball/latest/ug/whatissnowball.html) – Uses physical storage devices to transfer large amounts of data between Amazon S3 and your onsite data storage location at faster\-than\-internet speeds\.  |  Receive notifications for Snowball jobs\. For more information, see the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-event-sources.html)  | 

## Additional event sources<a name="sns-event-sources-aws-wide"></a>


| Source | Benefit of using with Amazon SNS | 
| --- | --- | 
|  [AWS IP address ranges](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)   |  Receive notifications of changes to AWS IP ranges\. For more information, see [AWS IP address ranges notifications](https://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html#subscribe-notifications) in the *Amazon Web Services General Reference*\.  | 

For more information on event\-driven computing, see the following sources:
+ [What is an Event\-Driven Architecture?](https://aws.amazon.com/event-driven-architecture/)
+ [Event\-Driven Computing with Amazon SNS and AWS Compute, Storage, Database, and Networking Services](https://aws.amazon.com/blogs/compute/event-driven-computing-with-amazon-sns-compute-storage-database-and-networking-services/) on the *AWS Compute Blog*
+ [Enriching Event\-Driven Architectures with AWS Event Fork Pipelines](https://aws.amazon.com/blogs/compute/enriching-event-driven-architectures-with-aws-event-fork-pipelines/) on the *AWS Compute Blog*