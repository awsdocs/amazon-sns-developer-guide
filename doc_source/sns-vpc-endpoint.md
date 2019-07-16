# Creating an Amazon VPC Endpoint for Amazon SNS<a name="sns-vpc-endpoint"></a>

To publish messages to your Amazon SNS topics from an Amazon VPC, create an interface VPC endpoint\. Then, you can publish messages to your topics while keeping the traffic within the network that you manage with the VPC\.

Use the following information to create the endpoint and test the connection between your VPC and Amazon SNS\. Or, for a walkthrough that helps you start from scratch, see [Tutorial: Publishing Amazon SNS Messages Privately from Amazon VPC](sns-vpc-tutorial.md)\.

## Creating the Endpoint<a name="sns-vpc-endpoint-create"></a>

You can create an Amazon SNS endpoint in your VPC using the AWS Management Console, the AWS CLI, an AWS SDK, the Amazon SNS API, or AWS CloudFormation\.

For information about creating and configuring an endpoint using the Amazon VPC console or the AWS CLI, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

**Note**  
When you create an endpoint, specify Amazon SNS as the service that you want your VPC to connect to\. In the Amazon VPC console, service names vary based on the region\. For example, if you choose US East \(N\. Virginia\), the service name is **com\.amazonaws\.us\-east\-1\.sns**\.

For information about creating and configuring an endpoint using AWS CloudFormation, see the [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpcendpoint.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpcendpoint.html) resource in the *AWS CloudFormation User Guide*\.

## Testing the Connection Between Your VPC and Amazon SNS<a name="sns-vpc-publish"></a>

After you create an endpoint for Amazon SNS, you can publish messages from your VPC to your Amazon SNS topics\. To test this connection, do the following:

1. Connect to an Amazon EC2 instance that resides in your VPC\. For information about connecting, see [Connect to Your Linux Instance](https://docs.aws.amazon.com/AWSEC2/latest/DeveloperGuide/AccessingInstances.html) or [Connecting to Your Windows Instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html) in the Amazon EC2 documentation\.

   For example, to connect to a Linux instance using an SSH client, run the following command from a terminal:

   ```
   $ ssh -i ec2-key-pair.pem ec2-user@instance-hostname
   ```

   Where:
   + *ec2\-key\-pair\.pem* is the file that contains the key pair that Amazon EC2 provided when you created the instance\.
   + *instance\-hostname* is the public hostname of the instance\. To get the hostname in the [Amazon EC2 console](https://console.aws.amazon.com/ec2): Choose **Instances**, select your instance, and find the value for **Public DNS \(IPv4\)**\.

1. From your instance, use the Amazon SNS [https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html) command with the AWS CLI\. You can send a simple message to a topic with the following command:

   ```
   $ aws sns publish --region aws-region --topic-arn sns-topic-arn --message "Hello"
   ```

   Where:
   + *aws\-region* is the AWS Region that the topic is located in\.
   + *sns\-topic\-arn* is the Amazon Resource Name \(ARN\) of the topic\. To get the ARN from the [Amazon SNS console](https://console.aws.amazon.com/sns): Choose **Topics**, find your topic, and find the value in the **ARN** column\.

   If the message is successfully received by Amazon SNS, the terminal prints a message ID, like the following:

   ```
   {
      "MessageId": "6c96dfff-0fdf-5b37-88d7-8cba910a8b64"
   }
   ```