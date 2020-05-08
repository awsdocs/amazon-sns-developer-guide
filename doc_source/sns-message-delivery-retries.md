# Message delivery retries<a name="sns-message-delivery-retries"></a>

Amazon SNS defines a *delivery policy* for each delivery protocol\. The delivery policy defines how Amazon SNS retries the delivery of messages when server\-side errors occur \(when the system that hosts the subscribed endpoint becomes unavailable\)\. When the delivery policy is exhausted, Amazon SNS stops retrying the delivery and discards the message—unless a dead\-letter queue is attached to the subscription\. For more information, see [Amazon SNS dead\-letter queues](sns-dead-letter-queues.md)\.

## Delivery protocols and policies<a name="delivery-policies-for-protocols"></a>

**Note**  
With the exception of HTTP/S, you can't change Amazon SNS\-defined delivery policies\. Only HTTP/S supports custom policies\. See [Creating an HTTP/S delivery policy](#creating-delivery-policy)\.
Amazon SNS applies jittering to delivery retries\. For more information, see the [Exponential Backoff and Jitter](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter/) post on the *AWS Architecture Blog*\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-message-delivery-retries.html)

## Delivery policy stages<a name="delivery-policy-stages"></a>

The following diagram shows the phases of a delivery policy\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/sns-delivery-policy-phases.png)

Each delivery policy is comprised of four phases\.

1. **Immediate Retry Phase \(No Delay\)** – This phase occurs immediately after the initial delivery attempt\. There is no delay between retries in this phase\.

1. **Pre\-Backoff Phase** – This phase follows the Immediate Retry Phase\. Amazon SNS uses this phase to attempt a set of retries before applying a backoff function\. This phase specifies the number of retries and the amount of delay between them\.

1. **Backoff Phase** – This phase controls the delay between retries by using the retry\-backoff function\. This phase sets a minimum delay, a maximum delay, and a retry\-backoff function that defines how quickly the delay increases from the minimum to the maximum delay\. The backoff function can be arithmetic, exponential, geometric, or linear\.

1. **Post\-Backoff Phase** – This phase follows the backoff phase\. It specifies a number of retries and the amount of delay between them\. This is the final phase\.

## Creating an HTTP/S delivery policy<a name="creating-delivery-policy"></a>

You can use a delivery policy and its four phases to define how Amazon SNS retries the delivery of messages to HTTP/S endpoints\. Amazon SNS lets you override the default retry policy for HTTP endpoints when you might, for example, want to customize the policy based your HTTP server's capacity\.

You can set your HTTP/S delivery policy as a JSON object at the subscription or topic level\. When you define the policy at the topic level, it applies to all HTTP/S subscriptions associated with the topic\.

You should customize your delivery policy according to your HTTP/S server's capacity\. You can set the policy as a topic attribute or a subscription attribute\. If all HTTP/S subscriptions in your topic target the same HTTP/S server, we recommend that you set the delivery policy as a topic attribute, so that it remains valid for all HTTP/S subscriptions in the topic\. Otherwise, you must compose a delivery policy for each HTTP/S subscription in your topic, according the capacity of the HTTP/S server that the policy targets\.

The following JSON object represents a delivery policy that instructs Amazon SNS to retry a failed HTTP/S delivery attempt, as follows:

1. 3 times immediately in the no\-delay phase

1. 2 times \(1 second apart\) in the pre\-backoff phase

1. 10 times \(with exponential backoff from 1 second to 60 seconds\)

1. 35 times \(60 seconds apart\)

Amazon SNS makes a total of 50 attempts before discarding the message\.

**Note**  
This delivery policy also instructs Amazon SNS to throttle deliveries to no more than 10 per second\.

```
{
  "healthyRetryPolicy": {
    "minDelayTarget": 1,
    "maxDelayTarget": 60,
    "numRetries": 50,
    "numNoDelayRetries": 3,
    "numMinDelayRetries": 2,
    "numMaxDelayRetries": 35,
    "backoffFunction": "exponential"
  },
  "throttlePolicy": {
    "maxReceivesPerSecond": 10
  }
}
```

The delivery policy is composed of a retry policy and a throttle policy\. In total, there are eight attributes in a delivery policy\.


| Policy  | Description | Constraint | 
| --- | --- | --- | 
| minDelayTarget | The minimum delay for a retry\.**Unit:** Seconds | 1 to maximum delay**Default:** 20 | 
| maxDelayTarget | The maximum delay for a retry\.**Unit:** Seconds | Minimum delay to 3,600**Default:** 20 | 
| numRetries | The total number of retries, including immediate, pre\-backoff, backoff, and post\-backoff retries\. | 0 to 100**Default:** 3 | 
| numNoDelayRetries | The number of retries to be done immediately, with no delay between them\. | 0 or greater**Default:** 0 | 
| numMinDelayRetries | The number of retries in the pre\-backoff phase, with the specified minimum delay between them\. | 0 or greater**Default:** 0 | 
| numMaxDelayRetries | The number of retries in the post\-backoff phase, with the maximum delay between them\. | 0 or greater**Default:** 0 | 
| backoffFunction | The model for backoff between retries\.  |  One of four options: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sns/latest/dg/sns-message-delivery-retries.html) **Default:** linear  | 
| maxReceivesPerSecond  | The maximum number of deliveries per second, per subscription\. | 1 or greater**Default:** No throttling | 

Amazon SNS uses the following formula to calculate the number of retries in the backoff phase:

```
numRetries - numNoDelayRetries - numMinDelayRetries - numMaxDelayRetries
```

You can use three parameters to control the frequency of retries in the backoff phase\.
+ `minDelayTarget` – Defines the delay associated with the first retry attempt in the backoff phase\.
+ `maxDelayTarget` – Defines the delay associated with the final retry attempt in the backoff phase\.
+ `backoffFunction` – Defines the algorithm that Amazon SNS uses to calculate the delays associated with all of the retry attempts between the first and last retries in the backoff phase\. You can use one of four retry\-backoff functions\.

The following diagram shows how each retry backoff function affects the delay associated with retries during the backoff phase: A delivery policy with the total number of retries set to 10, the minimum delay set to 5 seconds, and the maximum delay set to 260 seconds\. The vertical axis represents the delay in seconds associated with each of the 10 retries\. The horizontal axis represents the number of retries, from the first to the tenth attempt\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sns/latest/dg/images/backoff-graph.png)