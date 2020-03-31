# Register your mobile app with AWS<a name="mobile-push-send-register"></a>

 For Amazon SNS to send notification messages to mobile endpoints, whether it is direct or with subscriptions to a topic, you first need to register the app with AWS\. To register your mobile app with AWS, enter a name to represent your app, choose the platform that will be supported, and provide your credentials for the notification service platform\. After the app is registered with AWS, the next step is to create an endpoint for the app and mobile device\. The endpoint is then used by Amazon SNS for sending notification messages to the app and device\. 

**To register your mobile app with AWS**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Choose **Push notifications** from the left menu, then choose **Create platform application**\.

1. In the **Application name** box, enter a name to represent your app\.

   App names must be made up of only uppercase and lowercase ASCII letters, numbers, underscores, hyphens, and periods, and must be between 1 and 256 characters long\.

1. In the **Push notification platform** box, choose the platform that the app is registered with and then enter the appropriate credentials\. 
**Note**  
 If you are using one of the APNs platforms, then you can select **Choose file** to upload the \.p12 file \(exported from Keychain Access\) to Amazon SNS\.

1. After you have entered this information, then choose **Add New App**\. 

   This registers the app with Amazon SNS, which creates a platform application object for the selected platform and then returns a corresponding PlatformApplicationArn\.