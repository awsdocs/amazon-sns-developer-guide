# Internetwork traffic privacy<a name="sns-internetwork-traffic-privacy"></a>

An Amazon Virtual Private Cloud \(Amazon VPC\) endpoint for Amazon SNS is a logical entity within a VPC that allows connectivity only to Amazon SNS\. The VPC routes requests to Amazon SNS and routes responses back to the VPC\. The following sections provide information about working with VPC endpoints and creating VPC endpoint policies\.

If you use Amazon Virtual Private Cloud \(Amazon VPC\) to host your AWS resources, you can establish a private connection between your VPC and Amazon SNS\. With this connection, you can publish messages to your Amazon SNS topics without sending them through the public internet\.

Amazon VPC is an AWS service that you can use to launch AWS resources in a virtual network that you define\. With a VPC, you have control over your network settings, such the IP address range, subnets, route tables, and network gateways\. To connect your VPC to Amazon SNS, you define an *interface VPC endpoint*\. This type of endpoint enables you to connect your VPC to AWS services\. The endpoint provides reliable, scalable connectivity to Amazon SNS without requiring an internet gateway, network address translation \(NAT\) instance, or VPN connection\. For more information, see [Interface VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

The information in this section is for users of Amazon VPC\. For more information, and to get started with creating a VPC, see [Getting Started With Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/getting-started-ipv4.html) in the *Amazon VPC User Guide*\.

**Note**  
VPC endpoints don't allow you to subscribe an Amazon SNS topic to a private IP address\.

**Topics**
+ [Creating an Amazon VPC endpoint for Amazon SNS](sns-vpc-create-endpoint.md)
+ [Creating an Amazon VPC endpoint policy for Amazon SNS](sns-vpc-endpoint-policy.md)
+ [Publishing an Amazon SNS message from a VPC from Amazon VPC](sns-vpc-tutorial.md)