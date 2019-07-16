# Amazon Virtual Private Cloud Endpoints for Amazon SNS<a name="sns-publishing-to-topics-from-vpc"></a>

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC and Amazon SNS\. With this connection, you can publish messages to your Amazon SNS topics without sending them through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. To connect your VPC to Amazon SNS, you define an *interface VPC endpoint*\. This type of endpoint enables you to connect your VPC to AWS services\. The endpoint provides reliable, scalable connectivity to Amazon SNS without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [Interface VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

The information in this section is for users of Amazon VPC\. For more information, and to get started with creating a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\.

**Note**  
VPC endpoints don't allow you to subscribe an Amazon SNS topic to a private IP address\.

**Topics**
+ [Tutorial: Publishing Amazon SNS Messages Privately from Amazon VPC](sns-vpc-tutorial.md)
+ [Creating an Amazon VPC Endpoint for Amazon SNS](sns-vpc-endpoint.md)
+ [Creating an Amazon VPC Endpoint Policy for Amazon SNS](sns-vpc-endpoint-policy.md)