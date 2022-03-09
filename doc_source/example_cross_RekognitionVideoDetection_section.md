# Detect people and objects in a video with Amazon Rekognition using an AWS SDK<a name="example_cross_RekognitionVideoDetection_section"></a>

The following code examples show how to detect people and objects in a video with Amazon Rekognition\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 Use Amazon Rekognition to detect faces, objects, and people in videos by starting asynchronous detection jobs\. This example also configures Amazon Rekognition to notify an Amazon Simple Notification Service \(Amazon SNS\) topic when jobs complete and subscribes an Amazon Simple Queue Service \(Amazon SQS\) queue to the topic\. When the queue receives a message about a job, the job is retrieved and the results are output\.   
 This example is best viewed on GitHub\. For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/rekognition)\.   

**Services used in this example**
+ Amazon Rekognition
+ Amazon SNS
+ Amazon SQS

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SNS with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.