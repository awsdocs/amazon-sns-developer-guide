# Tutorial: Publishing Amazon SNS messages privately from Amazon VPC<a name="sns-vpc-tutorial"></a>

In this tutorial, you learn how to publish to an Amazon SNS topic while keeping the messages secure in a private network\. You publish a message from an Amazon EC2 instance that's hosted in Amazon Virtual Private Cloud \(Amazon VPC\)\. The message stays within the AWS network without traveling the public internet\. By publishing messages privately from a VPC, you can improve the security of the traffic between your applications and Amazon SNS\. This security is important when you publish personally identifiable information \(PII\) about your customers, or when your application is subject to market regulations\. For example, publishing privately is helpful if you have a healthcare system that must comply with the Health Insurance Portability and Accountability Act \(HIPAA\), or a financial system that must comply with the Payment Card Industry Data Security Standard \(PCI DSS\)\.

To complete this tutorial, you:
+ Use an AWS CloudFormation template to automatically create a temporary private network in your AWS account\.
+ Create a VPC endpoint that connects the VPC with Amazon SNS\.
+ Log in to an Amazon EC2 instance and publish a message privately to an Amazon SNS topic\.
+ Verify that the message was delivered successfully\.
+ Delete the resources that you created for this tutorial so that they don't remain in your AWS account\.

The following diagram depicts the private network that you create in your AWS account as you complete this tutorial:

![\[The architecture of the private network that you create with this tutorial.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-architecture.png)

This network consists of a VPC that contains an Amazon EC2 instance\. The instance connects to Amazon SNS through an *interface VPC endpoint*\. This type of endpoint connects to services that are powered by AWS PrivateLink\. With this connection established, you can log in to the Amazon EC2 instance and publish messages to the Amazon SNS topic, even though the network is disconnected from the public internet\. The topic fans out the messages that it receives to two subscribing AWS Lambda functions\. These functions log the messages that they receive in Amazon CloudWatch Logs\.

This tutorial takes about 20 minutes to complete\.

**Topics**
+ [Before you begin](#sns-vpc-tutorial-prereqs)
+ [Step 1: Create a key pair](#sns-vpc-tutorial-keypair)
+ [Step 2: Create resources](#sns-vpc-tutorial-resources)
+ [Step 3: Check the internet connection for your instance](#sns-vpc-tutorial-connection)
+ [Step 4: Create an endpoint](#sns-vpc-tutorial-endpoint)
+ [Step 5: Publish a message](#sns-vpc-tutorial-publish)
+ [Step 6: Verify](#sns-vpc-tutorial-verify)
+ [Step 7: Clean up](#sns-vpc-tutorial-delete)
+ [Related resources](#sns-vpc-tutorial-resources-related)

## Before you begin<a name="sns-vpc-tutorial-prereqs"></a>

Before you start this tutorial, you need an Amazon Web Services \(AWS\) account\. When you sign up, your account is automatically signed up for all services in AWS, including Amazon SNS and Amazon VPC\. If you haven't created an account already, go to [https://aws.amazon.com/](https://aws.amazon.com/), and then choose **Create a Free Account**\.

## Step 1: Create an Amazon EC2 key pair<a name="sns-vpc-tutorial-keypair"></a>

A *key pair* is used to log in to an Amazon EC2 instance\. It consists of a public key that's used to encrypt your login information, and a private key that's used to decrypt it\. When you create a key pair, you download a copy of the private key\. Later in this tutorial, you use the key pair to log in to an Amazon EC2 instance\. To log in, you specify the name of the key pair, and you provide the private key\.

**To create the key pair**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation menu on the left, find the **Network & Security** section\. Then, choose **Key Pairs**\.

1. Choose **Create Key Pair**\.

1. In the **Create Key Pair** window, for **Key pair name**, type **VPCE\-Tutorial\-KeyPair**\. Then, choose **Create**\.  
![\[The Create Key Pair window.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-key-pair.png)

1. The private key file is automatically downloaded by your browser\. Save it in a safe place\. Amazon EC2 gives the file an extension of `.pem`\. 

1. \(Optional\) If you're using an SSH client on a Mac or Linux computer to connect to your instance, use the `chmod` command to set the permissions of your private key file so that only you can read it:

   1. Open a terminal and navigate to the directory that contains the private key: 

      ```
      $ cd /filepath_to_private_key/
      ```

   1. Set the permissions using the following command:

      ```
      $ chmod 400 VPCE-Tutorial-KeyPair.pem
      ```

## Step 2: Create the AWS resources<a name="sns-vpc-tutorial-resources"></a>

To set up the infrastructure that supports this tutorial, you use an AWS CloudFormation *template*\. A template is a file that acts as a blueprint for building AWS resources, such as Amazon EC2 instances and Amazon SNS topics\. The template for this tutorial is provided on GitHub for you to download\. 

You provide the template to AWS CloudFormation, and AWS CloudFormation provisions the resources that you need as a *stack* in your AWS account\. A stack is a collection of resources that you manage as a single unit\. When you finish the tutorial, you can use AWS CloudFormation to delete all of the resources in the stack at once\. These resources don't remain in your AWS account, unless you want them to\.

The stack for this tutorial includes the following resources:
+ A VPC and the associated networking resources, including a subnet, a security group, an internet gateway, and a route table\.
+ An Amazon EC2 instance that's launched into the subnet in the VPC\.
+ An Amazon SNS topic\.
+ Two AWS Lambda functions\. These functions receive messages that are published to the Amazon SNS topic, and they log events in CloudWatch Logs\.
+ Amazon CloudWatch metrics and logs\.
+ An IAM role that allows the Amazon EC2 instance to use Amazon SNS, and an IAM role that allows the Lambda functions to write to CloudWatch logs\.

**To create the AWS resources**

1. Download the [template file](https://github.com/aws-samples/aws-sns-samples/blob/master/templates/SNS-VPCE-Tutorial-CloudFormation.template) from the GitHub website\.

1. Sign in to the [AWS CloudFormation console](https://console.aws.amazon.com/cloudformation)\.

1. Choose **Create Stack**\.

1. On the **Select Template** page, choose **Upload a template to Amazon S3**, choose the file, and choose **Next**\.

1. On the **Specify Details** page, specify stack and key names:

   1. For **Stack name**, type **VPCE\-Tutorial\-Stack**\.

   1. For **KeyName**, choose **VPCE\-Tutorial\-KeyPair**\.

   1. For **SSHLocation**, keep the default value of **0\.0\.0\.0/0**\.  
![\[The Specify Details page.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-stack-name.png)

   1. Choose **Next**\.

1. On the **Options** page, keep all of the default values, and choose **Next**\.

1. On the **Review** page, verify the stack details\.

1. Under **Capabilities**, acknowledge that AWS CloudFormation might create IAM resources with custom names\.

1. Choose **Create**\.

   The AWS CloudFormation console opens the **Stacks** page\. The VPCE\-Tutorial\-Stack has a status of **CREATE\_IN\_PROGRESS**\. In a few minutes, after the creation process completes, the status changes to **CREATE\_COMPLETE**\.  
![\[The AWS CloudFormation stack with a status of CREATE_COMPLETE.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-stack-create-complete.png)
**Tip**  
Choose the **Refresh** button to see the latest stack status\.

## Step 3: Confirm that your Amazon EC2 instance lacks internet access<a name="sns-vpc-tutorial-connection"></a>

The Amazon EC2 instance that was launched in your VPC in the previous step lacks internet access\. It disallows outbound traffic, and it's unable to publish messages to Amazon SNS\. Verify this by logging in to the instance\. Then, attempt to connect to a public endpoint, and attempt to message Amazon SNS\.

At this point in the tutorial, the publish attempt fails\. In a later step, after you create a VPC endpoint for Amazon SNS, your publish attempt succeeds\.

**To connect to your Amazon EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation menu on the left, find the **Instances** section\. Then, choose **Instances**\.

1. In the list of instances, select **VPCE\-Tutorial\-EC2Instance**\.

1. Copy the hostname that's provided in the **Public DNS \(IPv4\)** column\.  
![\[Details about the Amazon EC2 instance launched by AWS CloudFormation.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-instance-details.png)

1. Open a terminal\. From the directory that contains the key pair, connect to the instance using the following command, where *instance\-hostname* is the hostname that you copied from the Amazon EC2 console:

   ```
   $ ssh -i VPCE-Tutorial-KeyPair.pem ec2-user@instance-hostname
   ```

**To verify that the instance lacks internet connectivity**
+ In your terminal, attempt to connect to any public endpoint, such as amazon\.com:

  ```
  $ ping amazon.com
  ```

  Because the connection attempt fails, you can cancel at any time \(Ctrl \+ C on Windows or Command \+ C on macOS\)\.

**To verify that the instance lacks connectivity to Amazon SNS**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the navigation menu on the left, choose **Topics**\.

1. On the **Topics** page, copy the Amazon Resource Name \(ARN\) for the topic **VPCE\-Tutorial\-Topic**\.

1. In your terminal, attempt to publish a message to the topic:

   ```
   $ aws sns publish --region aws-region --topic-arn sns-topic-arn --message "Hello"
   ```

   Because the publish attempt fails, you can cancel at any time\.

## Step 4: Create an Amazon VPC endpoint for Amazon SNS<a name="sns-vpc-tutorial-endpoint"></a>

To connect the VPC to Amazon SNS, you define an interface VPC endpoint\. After you add the endpoint, you can log in to the Amazon EC2 instance in your VPC, and from there you can use the Amazon SNS API\. You can publish messages to the topic, and the messages are published privately\. They stay within the AWS network, and they don't travel the public internet\.

**Note**  
The instance still lacks access to other AWS services and endpoints on the internet\.

**To create the endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation menu on the left, choose **Endpoints**\.

1. Choose **Create Endpoint**\.

1. On the **Create Endpoint** page, for **Service category**, keep the default choice of **AWS services**\.

1. For **Service Name**, choose the service name for Amazon SNS\.

   The service names vary based on the chosen region\. For example, if you chose US East \(N\. Virginia\), the service name is **com\.amazonaws\.*us\-east\-1*\.sns**\.

1. For **VPC**, choose the VPC that has the name **VPCE\-Tutorial\-VPC**\.  
![\[The VPC menu on the Create Endpoint page.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-create-endpoint-vpc.png)

1. For **Subnets**, choose the subnet that has *VPCE\-Tutorial\-Subnet* in the subnet ID\.  
![\[The subnets on the Create Endpoints page.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-create-endpoint-subnet.png)

1. For **Enable Private DNS Name**, select **Enable for this endpoint**\.

1. For **Security group**, choose **Select security group**, and choose **VPCE\-Tutorial\-SecurityGroup**\.  
![\[The security groups on the Create Endpoints page.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-create-endpoint-security-group.png)

1. Choose **Create endpoint**\. The Amazon VPC console confirms that a VPC endpoint was created\.  
![\[The confirmation message displayed after you create an endpoint.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-create-endpoint-confirmation.png)

1. Choose **Close**\. 

   The Amazon VPC console opens the **Endpoints** page\. The new endpoint has a status of **pending**\. In a few minutes, after the creation process completes, the status changes to **available**\.  
![\[The VPC endpoint with a status of available.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-create-endpoint-status-available.png)

## Step 5: Publish a message to your Amazon SNS topic<a name="sns-vpc-tutorial-publish"></a>

Now that your VPC includes an endpoint for Amazon SNS, you can log in to the Amazon EC2 instance and publish messages to the topic\.

**To publish a message**

1. If your terminal is no longer connected to your Amazon EC2 instance, connect again:

   ```
   $ ssh -i VPCE-Tutorial-KeyPair.pem ec2-user@instance-hostname
   ```

1. Run the same command that you did previously to publish a message to your Amazon SNS topic\. This time, the publish attempt succeeds, and Amazon SNS returns a message ID:

   ```
   $ aws sns publish --region aws-region --topic-arn sns-topic-arn --message "Hello"
   
   {
       "MessageId": "5b111270-d169-5be6-9042-410dfc9e86de"
   }
   ```

## Step 6: Verify your message deliveries<a name="sns-vpc-tutorial-verify"></a>

When the Amazon SNS topic receives a message, it fans out the message by sending it to the two subscribing Lambda functions\. When these functions receive the message, they log the event to CloudWatch logs\. To verify that your message delivery succeeded, check that the functions were invoked, and check that the CloudWatch logs were updated\.

**To verify that the Lambda functions were invoked**

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. On the **Functions** page, choose **VPCE\-Tutorial\-Lambda\-1**\.

1. Choose **Monitoring**\.

1. Check the **Invocation count** graph\. This graph shows the number of times that the Lambda function has been run\.

   The invocation count matches the number of times you published a message to the topic\.  
![\[The Invocation count graph in the Lambda console.\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-lambda-invocation-count.png)

**To verify that the CloudWatch logs were updated**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation menu on the left, choose **Logs**\.

1. Check the logs that were written by the Lambda functions:

   1. Choose the **/aws/lambda/VPCE\-Tutorial\-Lambda\-1/** log group\.

   1. Choose the log stream\.

   1. Check that the log includes the entry `From SNS: Hello`\.  
![\[The CloudWatch log includes the entry "From SNS: Hello".\]](http://docs.aws.amazon.com/sns/latest/dg/images/vpce-tutorial-cloudwatch-log.png)

   1. Choose **Log Groups** at the top of the console to return the **Log Groups** page\. Then, repeat the preceding steps for the /aws/lambda/VPCE\-Tutorial\-Lambda\-2/ log group\.

Congratulations\! By adding an endpoint for Amazon SNS to a VPC, you were able to publish a message to a topic from within the network that's managed by the VPC\. The message was published privately without being exposed to the public internet\.

## Step 7: Clean up<a name="sns-vpc-tutorial-delete"></a>

Unless you want to retain the resources that you created for this tutorial, you can delete them now\. By deleting AWS resources that you're no longer using, you prevent unnecessary charges to your AWS account\. 

First, delete your VPC endpoint using the Amazon VPC console\. Then, delete the other resources that you created by deleting the stack in the AWS CloudFormation console\. When you delete a stack, AWS CloudFormation removes the stack's resources from your AWS account\.

**To delete your VPC endpoint**

1. Open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. In the navigation menu on the left, choose **Endpoints**\.

1. Select the endpoint that you created\.

1. Choose **Actions**, and then choose **Delete Endpoint**\.

1. In the **Delete Endpoint** window, choose **Yes, Delete**\.

   The endpoint status changes to **deleting**\. When the deletion completes, the endpoint is removed from the page\.

**To delete your AWS CloudFormation stack**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Select the stack **VPCE\-Tutorial\-Stack**\.

1. Choose **Actions**, and then choose **Delete Stack**\.

1. In the **Delete Stack** window, choose **Yes, Delete**\.

   The stack status changes to **DELETE\_IN\_PROGRESS**\. When the deletion completes, the stack is removed from the page\.

## Related resources<a name="sns-vpc-tutorial-resources-related"></a>

If you want to dive more deeply into the concepts introduced in this tutorial, see the following resources\.
+ [AWS Security Blog: Securing messages published to Amazon SNS with AWS PrivateLink ](http://aws.amazon.com/blogs/security/securing-messages-published-to-amazon-sns-with-aws-privatelink/)
+ [What Is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html)
+ [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html)
+ [What Is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
+ [AWS CloudFormation Concepts](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-whatis-concepts.html)