# Setting up access for Amazon SNS<a name="sns-setting-up"></a>

Before you can use Amazon SNS, you must complete the following steps\.

**Topics**
+ [Step 1: Create an AWS account and an IAM administrator user](#create-aws-account)
+ [Step 2: Create an IAM user and get your AWS credentials](#create-iam-user)
+ [Next steps](#next-steps-setting-up)

## Step 1: Create an AWS account and an IAM administrator user<a name="create-aws-account"></a>

To access any AWS service, you must first create an [AWS account](https://aws.amazon.com/)\. This is an Amazon account that can use AWS products\. You can use your AWS account to view your activity and usage reports and to manage authentication and access\.

1. Navigate to the [AWS home page](https://aws.amazon.com/), and then choose **Create an AWS Account**\.

1. Follow the instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

1. When you finish creating your AWS account, follow the instructions in the *IAM User Guide* to [create your first IAM administrator user and group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)\.

## Step 2: Create an IAM user and get your AWS credentials<a name="create-iam-user"></a>

To avoid using your IAM administrator user for Amazon SNS operations, it is a best practice to create an IAM user for each person who needs administrative access to Amazon SNS\.

To work with Amazon SNS, you need the `AmazonSNSFullAccess` policy and AWS credentials that are associated with your IAM user\. These credentials are comprised of an access key ID and a secret access key\. For more information, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html) in the *IAM User Guide* and [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *AWS General Reference*\.

1. Sign in to the [AWS Identity and Access Management console](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, **Add user**\.

1. Type a **User name**, such as `AmazonSNSAdmin`\.

1. Select **Programmatic access** and **AWS Management Console access**\.

1. Set a **Console password** and then choose **Next: Permissions**\.

1. On the **Set permissions** page, choose **Attach existing policies directly**\.

1. Type `AmazonSNS` into the filter, choose **AmazonSNSFullAccess**, and then choose **Next: Tags**\.

1. On the **Add tags \(optional\)** page, choose **Next: Review**\.

1. On the **Review** page, choose **Create user**\.

   The IAM user is created and the **Access key ID** is displayed, for example:

   **AKIAIOSFODNN7EXAMPLE**

1. To display your **Secret access key**, choose **Show**, for example:

   **wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY**
**Important**  
You can view or download your secret access key *only* when you create your credentials \(however, you can create new credentials at any time\)\.

1. To download your credentials, choose **Download \.csv**\. Keep this file in a secure location\.

## Next steps<a name="next-steps-setting-up"></a>

Now that you're prepared to work with Amazon SNS, [get started](sns-getting-started.md) by creating a topic, creating a subscription for the topic, publishing a message to the topic, and deleting the subscription and topic\.