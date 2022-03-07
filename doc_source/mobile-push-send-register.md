# Creating a platform application<a name="mobile-push-send-register"></a>

For Amazon SNS to send notification messages to mobile endpoints, whether directly or via subscriptions to a topic, you must first create a platform application\. After registering the app with AWS, the next step is to create an endpoint for the app and mobile device\. Amazon SNS then uses the endpoint for sending notification messages to the app and device\.

**To create a platform application**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the navigation pane, choose **Mobile**, and then choose **Push notifications**\.

1. In the **Platform applications** section, choose **Create platform application**\.

   For a list of AWS Regions where you can create mobile applications, see [Supported Regions for mobile applications](sns-mobile-push-supported-regions.md)\.

1. For **Application name**, enter a name to represent your app\.

   App names must be made up of only uppercase and lowercase ASCII letters, numbers, underscores, hyphens, and periods\. Names must also be 1–256 characters long\.

1. For **Push notification platform**, choose the platform that the app is registered with, and then enter the appropriate credentials\.
**Note**  
If you're using one of the Apple Push Notification Service \(APNs\) platforms, you can choose between [token\- or certificate\-based authentication](sns-apple-authentication-methods.md), then choose **Choose file** to upload the \.p8 or \.p12 file \(exported from Keychain Access\) to Amazon SNS\.

1. Choose **Create platform application**\.

   This registers the app with Amazon SNS, which creates a platform application object for the selected platform and then returns a corresponding PlatformApplicationArn\.