# Apple authentication methods<a name="sns-apple-authentication-methods"></a>

You can authorize Amazon SNS to send push notifications to your iOS or macOS app by providing information that identifies you as the developer of the app\. To authenticate, provide either a *key* or a *certificate* [when creating a platform application](https://docs.aws.amazon.com/sns/latest/api/API_SetPlatformApplicationAttributes.html), both of which you can get from your Apple Developer account\.

**Token signing key**  
A private signing key that Amazon SNS uses to sign Apple Push Notification Service \(APNs\) authentication tokens\.  
If you provide a signing key, Amazon SNS uses a token to authenticate with APNs for every push notification that you send\. With your signing key, you can send push notifications to APNs production and sandbox environments\.  
Your signing key doesn't expire, and you can use the same signing key for multiple apps\. For more information, see [Communicate with APNs using authentication tokens](https://help.apple.com/developer-account/#/deva05921840) in the **Developer Account Help** section of the Apple website\.

**Certificate**  
A TLS certificate that Amazon SNS uses to authenticate with APNs when you send push notifications\. You obtain the certificate from your Apple Developer account\.  
Certificates expire after one year\. When this happens, you must create a new certificate and provide it to Amazon SNS\. For more information, see [Establishing a Certificate\-Based Connection to APNs](https://developer.apple.com/documentation/usernotifications/setting_up_a_remote_notification_server/establishing_a_certificate-based_connection_to_apns) on the Apple Developer website\.

**To manage APNs settings using the AWS Management Console**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Under **Mobile**, choose **Push notifications**\.

1. Select the **Application** for which you would like to edit the APNs settings, and then choose **Edit**\.

1. On the **Edit** page, for **Authentication type**, choose either **Token** or **Certificate**\.

1. Depending on the authentication type that you choose, do one of the following:
   + If you choose **Token**, provide the following information from your Apple Developer account\. Amazon SNS requires this information to construct authentication tokens\.
     + **Signing key** – The authentication token signing key from your Apple Developer account, which you download as a \.p8 file\. Apple lets you download your signing key only once\.
     + **Signing key ID** – The ID that's assigned to your signing key\. Amazon SNS requires this information to construct authentication tokens\. To find this value in your Apple Developer account, choose **Certificates, IDs & Profiles**, and then choose your key in the **Keys** section\.
     + **Team identifier** – The ID that's assigned to your Apple Developer account team\. You can find this value on the **Membership** page\.
     + **Bundle identifier** – The ID that's assigned to your app\. To find this value, choose **Certificates, IDs & Profiles**, choose **App IDs** in the **Identifiers** section, and then choose your app\.
   + If you choose **Certificate**, provide the following information:
     + **SSL certificate** – The \.p12 file for your TLS certificate\. You can export this file from Keychain Access after you download and install your certificate from your Apple Developer account\.
     + **Certificate password** – If you assigned a password to your certificate, specify it here\.

1. When you finish, choose **Save changes**\.