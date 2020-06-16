# Using temporary security credentials with Amazon SNS<a name="sns-using-temporary-credentials"></a>

 In addition to creating IAM users with their own security credentials, IAM also enables you to grant temporary security credentials to any user allowing this user to access your AWS services and resources\. You can manage users who have AWS accounts; these users are IAM users\. You can also manage users for your system who do not have AWS accounts; these users are called federated users\. Additionally, "users" can also be applications that you create to access your AWS resources\. 

 You can use these temporary security credentials in making requests to Amazon SNS\. The API libraries compute the necessary signature value using those credentials to authenticate your request\. If you send requests using expired credentials Amazon SNS denies the request\. 

 For more information about IAM support for temporary security credentials, go to [Granting Temporary Access to Your AWS Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/TokenBasedAuth.html) in *Using IAM*\. 

**Example Using temporary security credentials to authenticate an Amazon SNS request**  
 The following example demonstrates how to obtain temporary security credentials to authenticate an Amazon SNS request\.   

```
http://sns.us-east-2.amazonaws.com/
?Name=My-Topic
&Action=CreateTopic
&Signature=gfzIF53exFVdpSNb8AiwN3Lv%2FNYXh6S%2Br3yySK70oX4%3D
&SignatureVersion=2
&SignatureMethod=HmacSHA256
&Timestamp=2010-03-31T12%3A00%3A00.000Z
&SecurityToken=SecurityTokenValue
&AWSAccessKeyId=Access Key ID provided by AWS Security Token Service
```