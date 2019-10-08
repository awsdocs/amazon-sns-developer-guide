# Setting Amazon SNS Delivery Retry Policies for HTTP/HTTPS Endpoints<a name="DeliveryPolicies"></a>

**Topics**
+ [Applying Delivery Policies to Topics and Subscriptions](#delivery-policy-topic-subscription)
+ [Setting the Maximum Receive Rate](#delivery-policy-maximum-receive-rate)
+ [Immediate Retry Phase](#delivery-policy-immediate-retry-phase)
+ [Pre\-Backoff Phase](#delivery-policy-pre-backoff-phase)
+ [Backoff Phase](#delivery-policy-backoff-phase)
+ [Post\-Backoff Phase](#delivery-policy-post-backoff-phase)

A successful Amazon SNS delivery to an HTTP/HTTPS endpoint sometimes requires more than one attempt\. This can be the case, for example, if the web server that hosts the subscribed endpoint is down for maintenance or is experiencing heavy traffic\. If an initial delivery attempt doesn't result in a successful response from the subscriber, Amazon SNS attempts to deliver the message again\. We call such an attempt a *retry*\. In other words, a retry is an attempted delivery that occurs after the initial delivery attempt\. 

Amazon SNS attempts a retry only after a failed delivery attempt\. Amazon SNS considers the following situations to indicate failed delivery attempts:
+ HTTP status codes 100 to 101 and 500 to 599 \(inclusive\)\.
**Note**  
Amazon SNS considers HTTP status codes 400 to 499 to indicate permanent delivery failure\.
+ A request timeout \(15 seconds\)\.
**Note**  
If a request timeout occurs, the next retry occurs at the specified interval after the timeout\. For example, if the retry interval is 20 seconds and a request times out, the start of the next request is 35 seconds after the start of the request that timed out\.
+ Any connection error such as connection timeout, endpoint unreachable, bad SSL certificate, etc\.

You can use delivery policies to control not only the total number of retries, but also the time delay between each retry\. You can specify up to 100 total retries distributed among four discrete phases\. The maximum lifetime of a message in the system is one hour\. This one hour limit cannot be extended by a delivery policy\.

![\[Graphic of the four Amazon SNS delivery policy phases.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-delivery-policy-phases.png)

1. **[Immediate Retry Phase](#delivery-policy-immediate-retry-phase)—**Also called the *no delay phase*, this phase occurs immediately after the initial delivery attempt\. The value you set for **Retries with no delay** determines the number of retries immediately after the initial delivery attempt\. There is no delay between retries in this phase\.

1. **[Pre\-Backoff Phase](#delivery-policy-pre-backoff-phase)—**The pre\-backoff phase follows the immediate retry phase\. Use this phase to create a set of retries that occur before a backoff function applies to the retries\. Use the **Minimum delay retries** setting to specify the number of retries in the Pre\-Backoff Phase\. You can control the time delay between retries in this phase using the **Minimum delay** setting\. 

1. **[Backoff Phase](#delivery-policy-backoff-phase)—**This phase is called the backoff phase because you can control the delay between retries in this phase using the retry backoff function\. Set the **Minimum delay** and the **Maximum delay**, and then choose a **Retry backoff function** to define how quickly the delay increases from the minimum delay to the maximum delay\.

1. **[Post\-Backoff Phase](#delivery-policy-post-backoff-phase)—**The post\-backoff phase follows the backoff phase\. Use the **Maximum delay retries** setting to specify the number of retries in the post\-backoff phase\. You can control the time delay between retries in this phase using the **Maximum delay** setting\. 

The backoff phase is the most commonly used phase\. If no delivery policies are set, the default is to retry three times in the backoff phase, with a time delay of 20 seconds between each retry\. The default value for both the **Minimum delay** and the **Maximum delay** is 20\. The default number of retries is 3, so the default retry policy calls for a total of 3 retries with a 20 second delay between each retry\. The following diagram shows the delay associated with each retry\.

![\[Graphic of Amazon SNS default delivery policy.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-delivery-policy-defaults.png)

To see how the retry backoff function affects the time delay between retries, you can set the maximum delay to 40 seconds and leave the remaining settings at their default values\. With this change, your delivery policy now specifies 3 retries during the backoff phase, a minimum delay of 20 seconds, and a maximum delay of 40 seconds\. Because the default backoff function is linear, the delay between messages increases at a constant rate over the course of the backoff phase\. Amazon SNS attempts the first retry after 20 seconds, the second retry after 30 seconds, and the final retry after 40 seconds\. The following diagram shows the delay associated with each retry\.

![\[Graphic of Amazon SNS default delivery policy with Maximum delay set to 40.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-delivery-policy-defaults-max40.png)

The maximum lifetime of a message in the system is one hour\. This one hour limit cannot be extended by a delivery policy\.

**Note**  
Only HTTP and HTTPS subscription types are supported by delivery policies\. Support for other Amazon SNS subscription types \(for example, email, Amazon SQS, and SMS\) is not currently available\. 

## Applying Delivery Policies to Topics and Subscriptions<a name="delivery-policy-topic-subscription"></a>

 You can apply delivery policies to Amazon SNS topics\. If you set a delivery policy on a topic, the policy applies to all of the topic's subscriptions\. The following diagram illustrates a topic with a delivery policy that applies to all three subscriptions associated with that topic\.

![\[Amazon SNS topic with three HTTP subscriptions and a topic delivery policy.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-http-diagram-topic-delivery-policy.png)

 You can also apply delivery policies to individual subscriptions\. If you assign a delivery policy to a subscription, the subscription\-level policy takes precedence over the topic\-level delivery policy\. In the following diagram, one subscription has a subscription\-level delivery policy whereas the two other subscriptions do not\. 

![\[Amazon SNS topic with three HTTP subscriptions, a topic delivery policy, and one subscription delivery policy.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-http-diagram-subscription-delivery-policy.png)

In some cases, you might want to ignore all subscription delivery policies so that your topic's delivery policy applies to all subscriptions even if a subscription has set its own delivery policy\. To configure Amazon SNS to apply your topic delivery policy to all subscriptions, choose **Ignore subscription override** in the **View/Edit Topic Delivery Policies** dialog box\. The following diagram shows a topic\-level delivery policy that applies to all subscriptions, even the subscription that has its own subscription delivery policy because subscription\-level policies have been specifically ignored\.

![\[Amazon SNS topic with three HTTP subscriptions, a topic delivery policy, and one subscription delivery policy that is ignored by Amazon SNS.\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-http-diagram-ignore-subscription-delivery-policy.png)

## Setting the Maximum Receive Rate<a name="delivery-policy-maximum-receive-rate"></a>

You can set the maximum number of messages per second that Amazon SNS sends to a subscribed endpoint\. Amazon SNS holds messages that are awaiting delivery for up to an hour\. After an hour, Amazon SNS discards the messages\.<a name="to-set-max-receive-rate-topic"></a>

**To set the maximum receive rate for a topic**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics** and then chose the name of a topic\.

1. On the ***MyTopic*** page, choose the **Edit**\.

1. On the **Edit *MyTopic*** page, expand the **Delivery retry policy \(HTTP/S\)** section and specify the **Maximum retry rate**\.

1. Choose **Save changes**\.

## Immediate Retry Phase<a name="delivery-policy-immediate-retry-phase"></a>

The immediate retry phase occurs directly after the initial delivery attempt\. This phase is also known as the No Delay phase because it happens with no time delay between the retries\. The default number of retries for this phase is 0\.

**To set the number of retries in the immediate retry phase**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics** and then choose a topic ARN\.

1. In the **Topic Details** section, choose **Edit topic delivery policy** from the **Other topic actions** drop\-down list\. 

1. In the **Retries with no delay** box, type an integer value\. 

1. Choose **Update policy** to save your changes\.

## Pre\-Backoff Phase<a name="delivery-policy-pre-backoff-phase"></a>

The pre\-backoff phase follows the immediate retry phase\. Use this phase if you want to create a set of one or more retries that happen before the backoff function affects the delay between retries\. In this phase, the time between retries is constant and is equal to the setting that you choose for the **Minimum delay**\. The **Minumum delay** setting affects retries in two phases—it applies to all retries in the pre\-backoff phase and serves as the initial time delay for retries in the backoff phase\. The default number of retries for this phase is 0\.

**To set the number of retries in the pre\-backoff phase**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics** and then choose a topic ARN\.

1. In the **Topic Details** section, choose **Edit topic delivery policy** from the **Other topic actions** drop\-down list\. 

1. In the **Minimum delay retries** box, type an integer value\. 

1. In the **Minimum delay** box, type an integer value to set the delay between messages in this phase\.

   The value you set must be less than or equal to the value you set for **Maximum delay**\. 

1. Choose **Update policy** to save your changes\.

## Backoff Phase<a name="delivery-policy-backoff-phase"></a>

The backoff phase is the only phase that applies by default\. You can control the number of retries in the backoff phase using **Number of retries**\. 

**Important**  
The value you choose for **Number of retries** represents the total number of retries, including the retries you set for **Retries with no delay**, **Minimum delay retries**, and **Maximum delay retries**\. 

You can control the frequency of the retries in the backoff phase with three parameters\. 
+ **Minimum delay—**The minimum delay defines the delay associated with the first retry attempt in the backoff phase\. 
+ **Maximum delay—**The maximum delay defines the delay associated with the final retry attempt in the backoff phase\.
+ **Retry backoff function—**The retry backoff function defines the algorithm that Amazon SNS uses to calculate the delays associated with all of the retry attempts between the first and last retries in the backoff phase\.

You can choose from four retry backoff functions\. 
+ Linear
+ Arithmetic
+ Geometric
+ Exponential

The following screen shot shows how each retry backoff function affects the delay associated with messages during the backoff period\. The vertical axis represents the delay in seconds associated with each of the 10 retries\. The horizontal axis represents the retry number\. The minimum delay is 5 seconds, and the maximum delay is 260 seconds\.

![\[Graph that displays four backoff retry functions.\]](http://docs.aws.amazon.com/sns/latest/dg/images/backoff-graph.png)

## Post\-Backoff Phase<a name="delivery-policy-post-backoff-phase"></a>

The post\-backoff phase is the final phase\. Use this phase if you want to create a set of one or more retries that happen after the backoff function affects the delay between retries\. In this phase, the time between retries is constant and is equal to the setting that you choose for the **Maximum delay**\. The Maximum delay setting affects retries in two phases—it applies to all retries in the post\-backoff phase and serves as the final time delay for retries in the backoff phase\. The default number of retries for this phase is 0\.

**To set the number of retries in the post\-backoff phase**

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/)\.

1. On the navigation panel, choose **Topics** and then choose a topic ARN\.

1. In the **Topic Details** section, choose **Edit topic delivery policy** from the **Other topic actions** drop\-down list\. 

1. In the **Maximum delay retries** box, type an integer value\. 

1. In the **Maximum delay** box, type an integer value to set the delay between messages in this phase\.

   The value you set must be greater than or equal to the value you set for **Minimum delay**\. 

1. Choose **Update policy** to save your changes\.