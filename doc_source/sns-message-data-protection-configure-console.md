# Creating data protection policies to secure message data \(Console\)<a name="sns-message-data-protection-configure-console"></a>

The number and size of Amazon SNS resources in an AWS account are limited\. For more information, see [Amazon Simple Notification Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html)\.

**To create a data protection policy together with an Amazon SNS topic \(Console\)**  
Use this option to create a new data protection policy together with a standard Amazon SNS topic:

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Choose a topic or create a new one\. For more details on creating topics, see [Creating an Amazon SNS topic](sns-create-topic.md)\.

1. On the **Create topic** page, in the **Details** section, choose **Standard**\.

   1. Enter a **Name** for the topic\.

   1. \(Optional\) Enter a **Display name** for the topic\.

1. Expand **Data protection policy**\.

1. Choose a **Configuration mode**:
   + **Basic** – Define a data protection policy using a simple menu\.
   + **Advanced** – Define a custom data protection policy using JSON\.

1. Choose the statement\(s\) that you'd like to add to your data protection policy\. You can add both **Deny** and **Audit** statement types to the same data protection policy\.

   1. **Add deny statement** – Configure which sensitive data to prevent from moving through your topic, and which principals to prevent from delivering that data\.

      1. Select the **Data direction ** of the messages to which this deny statement applies:
         + **Inbound messages** – Apply this deny statement to messages that are sent to the topic\.
         + **Outbound messages** – Apply this deny statement to messages that the topic delivers to subscription endpoints\.

      1. Select the **data identifiers** to define the sensitive data that you want to deny\.

      1. Select the **IAM principals** to which this deny statement applies\. You can apply it to **all AWS accounts**, or to **specific AWS accounts** or **IAM entities** \(account roots, roles, or users\) that use account IDs or IAM entity ARNs\. Separate multiple IDs or ARNs using a comma \( , \)\. The following [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html) principals are supported:
         + **IAM account principals** \(for example, arn:aws:iam::AWS\-account\-ID:root\)
         + **IAM role principals** \(for example, arn:aws:iam::AWS\-account\-ID:role/role\-name\)
         + **IAM user principals** \(for example, arn:aws:iam::AWS\-account\-ID:user/user\-name\)

      1. \(Optional\) Continue to add deny statements as needed\.

   1. **Add audit statement** – Configure which sensitive data to audit, what percentage of messages you want to audit for that data, and where to send audit logs\.
**Note**  
Only one audit statement is allowed per data protection policy or topic\.

      1. Select **data identifiers ** to define the sensitive data that you want to audit\.

      1. For **Audit sample rate**, enter the percentage of messages to audit for sensitive information, up to a maximum of 99%\.

      1. For **Audit destination**, select which AWS services to send the audit finding results, and enter a destination name for each AWS service that you use\. You can select from the following Amazon Web Services:
         + **Amazon CloudWatch** – CloudWatch Logs is the AWS standard logging solution\. Using CloudWatch Logs, you can perform log analytics using Logs Insights \([see samples here](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html)\) and create metrics and alarms\. CloudWatch Logs is where many services publish logs, which makes it easier to aggregate all logs using one solution\. For information about Amazon CloudWatch, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)\.
         + **Amazon Kinesis Data Firehose** – Kinesis Data Firehose satisfies the demands for real\-time streaming to Splunk, OpenSearch, and Amazon Redshift for further log analytics\. For information about Amazon Kinesis Data Firehose, see the [Amazon Kinesis Data Firehose User Guide](https://docs.aws.amazon.com/firehose/latest/dev/what-is-this-service.html)\.
         + **Amazon Simple Storage Service** – Amazon S3 is an economical log destination for archival purposes\. You may be required to retain logs for a period of years\. In this case, you can put logs into Amazon S3 to save costs\. For information about Amazon Simple Storage Service, see the [Amazon Simple Storage Service User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)\.