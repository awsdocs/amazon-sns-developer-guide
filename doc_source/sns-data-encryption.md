# Data encryption<a name="sns-data-encryption"></a>

Data protection refers to protecting data while in\-transit \(as it travels to and from Amazon SNS\) and at rest \(while it is stored on disks in Amazon SNS data centers\)\. You can protect data in transit using Secure Sockets Layer \(SSL\) or client\-side encryption\. You can protect data at rest by requesting Amazon SNS to encrypt your messages before saving them to disk in its data centers and then decrypt them when the messages are received\.

**Topics**
+ [Encryption at rest](sns-server-side-encryption.md)
+ [Key management](sns-key-management.md)