# Managing phone numbers and SMS subscriptions<a name="sms_manage"></a>

Amazon SNS provides several options for managing who receives SMS messages from your account\. With a limited frequency, you can opt in phone numbers that have opted out of receiving SMS messages from your account\. To stop sending messages to SMS subscriptions, you can remove subscriptions or the topics that publish to them\.

**Topics**
+ [Opting out of receiving SMS messages](#sms_manage_optout)
+ [Managing phone numbers and subscriptions \(console\)](#sms_manage_console)
+ [Managing phone numbers and subscriptions \(AWS SDKs\)](#sms_manage_sdk)

## Opting out of receiving SMS messages<a name="sms_manage_optout"></a>

Where required by local laws and regulations \(such as the US and Canada\), SMS recipients can use their devices to opt out by replying to the message with any of the following: 
+ ARRET \(French\)
+ CANCEL
+ END
+ OPT\-OUT
+ OPTOUT
+ QUIT
+ REMOVE
+ STOP
+ TD
+ UNSUBSCRIBE

To opt out, the recipient must reply to the same long code or short code that Amazon SNS used to deliver the message\. After opting out, the recipient will no longer receive SMS messages delivered from your AWS account unless you opt in the phone number\.

If the phone number is subscribed to an Amazon SNS topic, opting out does not remove the subscription, but SMS messages will fail to deliver to that subscription unless you opt in the phone number\.

## Managing phone numbers and subscriptions \(console\)<a name="sms_manage_console"></a>

You can use the Amazon SNS console to control which phone numbers receive SMS messages from your account\.

### Opting in a phone number that has been opted out<a name="sms_manage_optout_console"></a>

You can view which phone numbers have been opted out of receiving SMS messages from your account, and you can opt in these phone numbers to resume sending messages to them\.

You can opt in a phone number only once every 30 days\.

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. In the console menu, set the region selector to a [region that supports SMS messaging](sns-supported-regions-countries.md)\.

1. On the navigation panel, choose **Text messaging \(SMS\)**\.

1. On the **Text messaging \(SMS\)** page, choose **View opted out phone numbers**\. The **Opted out phone numbers** page displays the opted out phone numbers\.

1. Select the check box for the phone number that you want to opt in, and choose **Opt in**\. The phone number is no longer opted out and will receive SMS messages that you send to it\.

### Deleting an SMS subscription<a name="sms_manage_subscriptions_console"></a>

Delete an SMS subscription to stop sending SMS messages to that phone number when you publish to your topics\.

1. On the navigation panel, choose **Subscriptions**\.

1. Select the check boxes for the subscriptions that you want to delete\. Then choose **Actions**, and choose **Delete Subscriptions**\.

1. In the **Delete** window, choose **Delete**\. Amazon SNS deletes the subscription and displays a success message\.

### Deleting a topic<a name="sms_manage_topic_console"></a>

Delete a topic when you no longer want to publish messages to its subscribed endpoints\.

1. On the navigation panel, choose **Topics**\.

1. Select the check boxes for the topics that you want to delete\. Then choose **Actions**, and choose **Delete Topics**\.

1. In the **Delete** window, choose **Delete**\. Amazon SNS deletes the topic and displays a success message\.

## Managing phone numbers and subscriptions \(AWS SDKs\)<a name="sms_manage_sdk"></a>

You can use the AWS SDKs to make programmatic requests to Amazon SNS and manage which phone numbers can receive SMS messages from your account\.

**Note**  
Remember to configure your AWS credentials before using the SDK\. For more information, see [AWS SDK for \.NET Developer Guide](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) or [AWS SDK for Java V2 Developer Guide](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html)

### Viewing all opted out phone numbers<a name="sms_view_optout_sdk"></a>

To view all opted out phone numbers, submit a `ListPhoneNumbersOptedOut` request with the Amazon SNS API\.

Or, you can use the Amazon SNS clients in the AWS SDKs, as shown by the following examples:

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you can use the `listPhoneNumbersOptedOut` method of the `AmazonSNSClient` class:

```
public static void main(String[] args) {
	AmazonSNSClient snsClient = new AmazonSNSClient();
	listOptOut(snsClient);
}

public static void listOptOut(AmazonSNSClient snsClient) {
	String nextToken = null;
	do {
		ListPhoneNumbersOptedOutResult result = snsClient
				.listPhoneNumbersOptedOut(new ListPhoneNumbersOptedOutRequest()
						.withNextToken(nextToken));
		nextToken = result.getNextToken();
		for (String phoneNum : result.getPhoneNumbers()) {
			System.out.println(phoneNum);
		}
	} while (nextToken != null);
}
```

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET, you can use the `ListPhoneNumbersOptedOut` method of the `AmazonSimpleNotificationServiceClient` class:

```
public static void Main(String[] args) 
{
    AmazonSimpleNotificationServiceClient snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
    ListOptOut(snsClient);
}

public static void ListOptOut(AmazonSimpleNotificationServiceClient snsClient)
{
    String nextToken = null;
    do
    {
        ListPhoneNumbersOptedOutRequest listRequest = new ListPhoneNumbersOptedOutRequest { NextToken = nextToken };
        ListPhoneNumbersOptedOutResponse listResponse = snsClient.ListPhoneNumbersOptedOut(listRequest);
        nextToken = listResponse.NextToken;
        foreach (String phoneNum in listResponse.PhoneNumbers)
            Console.WriteLine(phoneNum);
    } while (nextToken != null);
}
```

------

Amazon SNS returns a paginated response, so this example repeats the request each time Amazon SNS returns a next token\. When you run this example, it displays a list of all opted out phone numbers in the console output window of your IDE\.

### Checking whether a phone number is opted out<a name="sms_check_optout_sdk"></a>

To check whether a phone number is opted out, submit a `CheckIfPhoneNumberIsOptedOut` request with the Amazon SNS API\.

Or, you can use the Amazon SNS clients in the AWS SDKs, as shown by the following examples:

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you can use the `checkIfPhoneNumberIsOptedOut` method of the `AmazonSNSClient` class:

```
CheckIfPhoneNumberIsOptedOutRequest request = new CheckIfPhoneNumberIsOptedOutRequest().withPhoneNumber(phoneNumber);
System.out.println(snsClient.checkIfPhoneNumberIsOptedOut(request));
```

When you run this example, a true or false result is displayed in the console output window of your IDE:

```
{IsOptedOut: false}
```

------
#### [ AWS SDK for \.NET ]

Using the AWS SDK for \.NET, you can use the `CheckIfPhoneNumberIsOptedOut` method of the `AmazonSimpleNotificationServiceClient` class:

```
CheckIfPhoneNumberIsOptedOutRequest request = new CheckIfPhoneNumberIsOptedOutRequest { PhoneNumber = phoneNumber };
Console.WriteLine(snsClient.CheckIfPhoneNumberIsOptedOut(request).IsOptedOut);
```

When you run this example, a true or false result is displayed in the console output window of your IDE:

```
false
```

------

### Opting in a phone number that has been opted out<a name="sms_manage_optin_sdk"></a>

To opt in a phone number, submit an `OptInPhoneNumber` request with the Amazon SNS API\.

Or, you can use the Amazon SNS clients in the AWS SDKs, as shown by the following examples:

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you can use the `optInPhoneNumber` method of the `AmazonSNSClient` class:

```
snsClient.optInPhoneNumber(new OptInPhoneNumberRequest().withPhoneNumber(phoneNumber));
```

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET, you can use the `OptInPhoneNumber` method of the `AmazonSimpleNotificationServiceClient` class:

```
snsClient.OptInPhoneNumber(new OptInPhoneNumberRequest { PhoneNumber = phoneNumber});
```

------

You can opt in a phone number only once every 30 days\.

### Deleting an SMS subscription<a name="sms_manage_subscriptions_sdk"></a>

To delete an SMS subscription from an Amazon SNS topic, get the subscription ARN by submitting a `ListSubscriptions` request with the Amazon SNS API, and then pass the ARN to an `Unsubscribe` request\.

Or, you can use the Amazon SNS clients in the AWS SDKs, as shown by the following examples:

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you can get your subscription ARNs using the `listSubscriptions` method of the `AmazonSNSClient` class:

```
ListSubscriptionsResult result = snsClient.listSubscriptions();
for (Subscription sub : result.getSubscriptions()) {
	System.out.println(sub);
}
```

You can delete a subscription by passing its ARN as a string argument to the `unsubscribe` method:

```
snsClient.unsubscribe(subscriptionArn);
```

------
#### [ AWS SDK for \.NET ]

With the AWS SDK for \.NET, use code like the following:

```
ListSubscriptionsResponse response = snsClient.ListSubscriptions();
// find the subscriptionAwn you want
foreach (Subscription sub in response.Subscriptions)
    Console.WriteLine(sub.SubscriptionArn);
// unsubscribe
snsClient.Unsubscribe(subscriptionArn);
```

------

### Deleting a topic<a name="sms_manage_topic_sdk"></a>

To delete a topic and all of its subscriptions, get the topic ARN by submitting a `ListTopics` request with the Amazon SNS API, and then pass the ARN to the `DeleteTopic` request\.

Or, you can use the Amazon SNS clients in the AWS SDKs, as shown by the following examples:

------
#### [ AWS SDK for Java ]

With the AWS SDK for Java, you can get your topic ARNs using the `listTopics` method of the `AmazonSNSClient` class:

```
ListTopicsResult result = snsClient.listTopics();
for (Topic t : result.getTopics()) {
	System.out.println(t);
}
```

You can delete a topic by passing its ARN as a string argument to the `deleteTopic` method:

```
snsClient.deleteTopic(topicArn);
```

------
#### [ AWS SDK for \.NET ]

Using the AWS SDK for \.NET, use code like the following:

```
ListTopicsResponse restopics= snsClient.ListTopics();
// find the topicAwn you want
foreach (Topic t in restopics.Topics)
    Console.WriteLine(t.TopicArn);
// delete
snsClient.DeleteTopic(topicArn);
```

------