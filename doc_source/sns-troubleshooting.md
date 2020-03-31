# Troubleshooting Amazon SNS topics<a name="sns-troubleshooting"></a>

This section provides information about troubleshooting Amazon SNS topics\.

## Troubleshooting Amazon SNS topics using AWS X\-Ray<a name="sns-troubleshooting-using-x-ray"></a>

AWS X\-Ray collects data about requests that your application serves, and lets you view and filter data to identify potential issues and opportunities for optimization\. For any traced request to your application, you can see detailed information about the request, the response, and the calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs\.

You can use X\-Ray with Amazon SNS to trace and analyze the messages that travel through your application\. You can use the AWS Management Console to view the map of connections between Amazon SNS and other services that your application uses\. You can also use the console to view metrics such as average latency and failure rates\. For more information, see [Amazon SNS and AWS X\-Ray](https://docs.aws.amazon.com/xray/latest/devguide/xray-services-sns.html) in the *AWS X\-Ray Developer Guide*\.