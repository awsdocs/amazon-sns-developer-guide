# Send a direct message to a mobile device<a name="mobile-push-send-directmobile"></a>

You can send Amazon SNS push notification messages directly to an endpoint which represents an application on a mobile device\. 

**To send a direct message**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Mobile**, **Push notifications**\.

1. On the **Mobile push notifications** page, in the the **Platform applications** section, choose the name of the application, for example ***MyApp***\.

1. On the ***MyApp*** page, in the **Endpoints** section, choose an endpoint and then choose **Publish message**\.

1. On the **Publish message to endpoint** page, enter the message that will appear in the application on the mobile device and then choose **Publish message**\.

   Amazon SNS sends the notification message to the platform notification service which, in turn, sends the message to the application\.