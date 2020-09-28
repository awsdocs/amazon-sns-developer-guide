# Step 6: Send messages to the HTTP/HTTPS endpoint<a name="SendMessageToHttp.publish"></a>

You can send a message to a topic's subscriptions by publishing to the topic\. To publish to a topic, you can use the Amazon SNS console, the `[sns\-publish](https://docs.aws.amazon.com/cli/latest/reference/sns/publish.html)` CLI command, or the `[Publish](https://docs.aws.amazon.com/sns/latest/api/API_Publish.html)` API\.

If you followed [Step 1](SendMessageToHttp.prepare.md), the code that you deployed at your endpoint should process the notification\.

**To publish to a topic using the Amazon SNS console**

1. Using the credentials of the AWS account or IAM user with permission to publish to the topic, sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics** and then choose a topic\.

1. Choose the **Publish message** button\.

1. In the **Subject** box, enter a subject \(for example, **Testing publish to my endpoint**\)\.

1. In the **Message** box, enter some text \(for example, **Hello world\!**\), and choose **Publish message**\.

    The following message appears: Your message has been successfully published\.