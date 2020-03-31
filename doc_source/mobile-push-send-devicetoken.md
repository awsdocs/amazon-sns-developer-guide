# Add device tokens or registration IDs<a name="mobile-push-send-devicetoken"></a>

When you first register an app and mobile device with a notification service, such as Apple Push Notification Service \(APNs\) and Firebase Cloud Messaging \(FCM\), device tokens or registration IDs are returned from the notification service\. When you add the device tokens or registration IDs to Amazon SNS, they are used with the `PlatformApplicationArn` API to create an endpoint for the app and device\. When Amazon SNS creates the endpoint, an `EndpointArn` is returned\. The `EndpointArn` is how Amazon SNS knows which app and mobile device to send the notification message to\. 

 You can add device tokens and registration IDs to Amazon SNS using the following methods: 
+ Manually add a single token to AWS using the AWS Management Console
+ Migrate existing tokens from a CSV file to AWS using the AWS Management Console 
+ Upload several tokens using the `CreatePlatformEndpoint` API 
+ Register tokens from devices that will install your apps in the future

**To manually add a device token or registration ID**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Choose **Apps**, choose your app, and then choose **Add Endpoints**\.

1. In the **Endpoint Token** box, enter either the token ID or registration ID, depending on which notification service\. For example, with ADM and FCM you enter the registration ID\.

1. \(Optional\) In the **User Data** box, enter arbitrary information to associate with the endpoint\. Amazon SNS does not use this data\. The data must be in UTF\-8 format and less than 2KB\.

1. Finally, choose **Add Endpoints**\.

   Now with the endpoint created, you can either send messages directly to a mobile device or send messages to mobile devices that are subscribed to a topic\.

**To migrate existing tokens from a CSV file to AWS**

 You can migrate existing tokens contained in a CSV file\. The CSV file cannot be larger than 2MB\. When migrating several tokens, it is recommended to use the `CreatePlatformEndpoint` API\. Each of the tokens in the CSV file must be followed by a newline\. For example, your CSV file should look similar to the following: 

```
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz01,"User data with spaces requires quotes"
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz04,"Data,with,commas,requires,quotes"
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz02,"Quoted data requires ""escaped"" quotes"
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz03,"{""key"": ""json is allowed"", ""value"":""endpoint"", ""number"": 1}"
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz05,SimpleDataNoQuotes
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz06,"The following line has no user data"
amzn1.adm-registration.v1.XpvSSUk0Rc3hTVVV--TOKEN--KMTlmMWxwRkxMaDNST2luZz07
APBTKzPGlCyT6E6oOfpdwLpcRNxQp5vCPFiFeru9oZylc22HvZSwQTDgmmw9WdNlXMerUPxmpX0w1,"Different token style"
```

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. Choose **Apps**, choose your app, and then choose **Add Endpoints**\. 

1. Choose **Migrate existing tokens over to AWS**, choose **Choose File**, choose your CSV file, and then choose **Add Endpoints**\. 

**To upload several tokens using the `CreatePlatformEndpoint` API**

 The following steps show how to use the sample Java app \(`bulkupload` package\) provided by AWS to upload several tokens \(device tokens or registration IDs\) to Amazon SNS\. You can use this sample app to help you get started with uploading your existing tokens\. 
**Note**  
 The following steps use the Eclipse Java IDE\. The steps assume you have installed the AWS SDK for Java and you have the AWS security credentials for your AWS account\. For more information, see [AWS SDK for Java](http://aws.amazon.com/sdkforjava/)\. For more information about credentials, see [How Do I Get Security Credentials?](https://docs.aws.amazon.com/general/latest/gr/getting-aws-sec-creds.html) in the *AWS General Reference*\. 

1.  Download and unzip the [snsmobilepush\.zip](samples/snsmobilepush.zip) file\. 

1. Create a new Java Project in Eclipse\.

1.  Import the `SNSSamples` folder to the top\-level directory of the newly created Java Project\. In Eclipse, right\-choose the name of the Java Project and then choose **Import**, expand **General**, choose **File System**, choose **Next**, browse to the `SNSSamples` folder, choose **OK**, and then choose **Finish**\. 

1.  Download a copy of the [OpenCSV library](http://sourceforge.net/projects/opencsv/) and add it to the Build Path of the `bulkupload` package\.

1.  Open the `BulkUpload.properties` file contained in the `bulkupload` package\. 

1. Add the following to `BulkUpload.properties`:
   + The `ApplicationArn` to which you want to add endpoints\.
   + The absolute path for the location of your CSV file containing the tokens\.
   + The names for CSV files \(such as `goodTokens.csv` and `badTokens.csv`\) to be created for logging the tokens that Amazon SNS parses correctly and those that fail\.
   + \(Optional\) The characters to specify the delimiter and quote in the CSV file containing the tokens\.
   + \(Optional\) The number of threads to use to concurrently create endpoints\. The default is 1 thread\.

   Your completed `BulkUpload.properties` should look similar to the following:

   ```
   applicationarn:arn:aws:sns:us-west-2:111122223333:app/FCM/fcmpushapp
   csvfilename:C:\\mytokendirectory\\mytokens.csv
   goodfilename:C:\\mylogfiles\\goodtokens.csv
   badfilename:C:\\mylogfiles\\badtokens.csv
   delimiterchar:' 
   quotechar:"
   numofthreads:5
   ```

1.  Run the BatchCreatePlatformEndpointSample\.java application to upload the tokens to Amazon SNS\. 

    In this example, the endpoints that were created for the tokens that were uploaded successfully to Amazon SNS would be logged to `goodTokens.csv`, while the malformed tokens would be logged to `badTokens.csv`\. In addition, you should see STD OUT logs written to the console of Eclipse, containing content similar to the following: 

   ```
   <1>[SUCCESS] The endpoint was created with Arn arn:aws:sns:us-west-2:111122223333:app/FCM/fcmpushapp/165j2214-051z-3176-b586-138o3d420071
   <2>[ERROR: MALFORMED CSV FILE] Null token found in /mytokendirectory/mytokens.csv
   ```

**To register tokens from devices that will install your apps in the future**

You can use one of the following two options:
+ **Use the Amazon Cognito service:** Your mobile app will need credentials to create endpoints associated with your Amazon SNS platform application\. We recommend that you use temporary credentials that expire after a period of time\. For most scenarios, we recommend that you use Amazon Cognito to create temporary security credentials\. For more information, see the *[Amazon Cognito Developer Guide](https://docs.aws.amazon.com/cognito/latest/developerguide/)* \. If you would like to be notified when an app registers with Amazon SNS, you can register to receive an Amazon SNS event that will provide the new endpoint ARN\. You can also use the `ListEndpointByPlatformApplication` API to obtain the full list of endpoints registered with Amazon SNS\. 
+ **Use a proxy server:** If your application infrastructure is already set up for your mobile apps to call in and register on each installation, you can continue to use this setup\. Your server will act as a proxy and pass the device token to Amazon SNS mobile push notifications, along with any user data you would like to store\. For this purpose, the proxy server will connect to Amazon SNS using your AWS credentials and use the `CreatePlatformEndpoint` API call to upload the token information\. The newly created endpoint Amazon Resource Name \(ARN\) will be returned, which your server can store for making subsequent publish calls to Amazon SNS\. 