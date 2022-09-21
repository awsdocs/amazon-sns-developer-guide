# Sensitive data types: Credentials<a name="sns-message-data-protection-sensitive-data-types-credentials"></a>

The following table lists and describes the types of credentials that Amazon SNS can detect using managed data identifiers\.


| Detection type | Managed data identifier ID | Keyword required | Countries and regions | 
| --- | --- | --- | --- | 
| AWS secret access key | AwsSecretKey | aws\_secret\_access\_key, credentials, secret access key, secret key, set\-awscredential |  Any  | 
| OpenSSH private key | OpenSshPrivateKey | No |  Any  | 
| PGP private key | PgpPrivateKey | No |  Any  | 
| Public\-Key Cryptography Standard \(PKCS\) private key | PkcsPrivateKey | No |  Any  | 
| PuTTY private key | PuttyPrivateKey | No |  Any  | 

## Data identifier ARNs for credential data types<a name="sns-message-data-protection-credentials-arns"></a>

The following lists the Amazon Resource Names \(ARNs\) for the data identifiers that you can add to your data protection policies\.
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/AwsSecretKey
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/OpenSshPrivateKey
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/PgpPrivateKey
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/PkcsPrivateKey
  ```
+ 

  ```
  arn:aws:dataprotection::aws:data-identifier/PuttyPrivateKey
  ```