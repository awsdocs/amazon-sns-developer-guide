# Infrastructure security in Amazon SNS<a name="sns-infrastructure-security"></a>

As a managed service, Amazon SNS is protected by the AWS global network security procedures described in the [Amazon Web Services: Overview of Security Processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

Use AWS API actions to access Amazon SNS through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with Perfect Forward Secrecy \(PFS\), such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\.

You must sign requests using both an access key ID and a secret access key associated with an IAM principal\. Alternatively, you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials for signing requests\.

You can call these API actions from any network location, but Amazon SNS supports resource\-based access policies, which can include restrictions based on the source IP address\. You can also use Amazon SNS policies to control access from specific Amazon VPC endpoints or specific VPCs\. This effectively isolates network access to a given Amazon SNS queue from only the specific VPC within the AWS network\. For more information, see [Restrict publication to an Amazon SNS topic only from a specific VPC endpoint](sns-access-policy-use-cases.md#sns-restrict-publication-only-from-specified-vpc-endpoint)\.