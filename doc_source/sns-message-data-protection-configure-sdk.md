# Creating data protection policies to secure message data \(SDK\)<a name="sns-message-data-protection-configure-sdk"></a>

The number and size of Amazon SNS resources in an AWS account are limited\. For more information, see [Amazon Simple Notification Service endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sns.html)\.

## Creating data protection policies \(AWS SDK\)<a name="create-policies-sdk"></a>

You can create an Amazon SNS data protection policy using the AWS SDK\.

**To create a data protection policy together with an Amazon SNS topic \(AWS SDK\)**  
Use the following options to create a new data protection policy together with a standard Amazon SNS topic:

------
#### [ Java ]

```
/**
 * For information regarding CreateTopic see this documentation topic:
 *
 * https://docs.aws.amazon.com/code-samples/latest/catalog/javav2-sns-src-main-java-com-example-sns-CreateTopic.java.html
 */

public static String createSNSTopicWithDataProtectionPolicy(SnsClient snsClient, String topicName, String dataProtectionPolicy) {

    try {
        CreateTopicRequest request = CreateTopicRequest.builder()
                .name(topicName)
                .dataProtectionPolicy(dataProtectionPolicy)
                .build();

        CreateTopicResponse result = snsClient.createTopic(request);
        return result.topicArn();
    } catch (SnsException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
    return "";
}
```

------
#### [ JavaScript ]

```
// Import required AWS SDK clients and commands for Node.js
import {CreateTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { Name: "TOPIC_NAME", DataProtectionPolicy: "DATA_PROTECTION_POLICY" };

const run = async () => {
  try {
    const data = await snsClient.send(new CreateTopicCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

------

**To create or retrieve a data protection policy for an existing Amazon SNS topic \(AWS SDK\)**  
Use the following options to create or retrieve a new data protection policy together with a standard Amazon SNS topic:

------
#### [ Java ]

```
public static void putDataProtectionPolicy(SnsClient snsClient, String topicName, String dataProtectionPolicy) {

    try {
        PutDataProtectionPolicyRequest request = PutDataProtectionPolicyRequest.builder()
                .resourceArn(topicName)
                .dataProtectionPolicy(dataProtectionPolicy)
                .build();

        PutDataProtectionPolicyResponse result = snsClient.putDataProtectionPolicy(request);
        System.out.println("\n\nStatus was " + result.sdkHttpResponse().statusCode() 
                + "\n\nTopic " + request.resourceArn()
                + " DataProtectionPolicy " + request.dataProtectionPolicy());
    } catch (SnsException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}


public static void getDataProtectionPolicy(SnsClient snsClient, String topicName) {

    try {
        GetDataProtectionPolicyRequest request = GetDataProtectionPolicyRequest.builder()
                .resourceArn(topicName)
                .build();
        
        GetDataProtectionPolicyResponse result = snsClient.getDataProtectionPolicy(request);
        
        System.out.println("\n\nStatus is " + result.sdkHttpResponse().statusCode() 
        + "\n\nDataProtectionPolicy: \n\n" + result.dataProtectionPolicy());
    } catch (SnsException e) {
        System.err.println(e.awsErrorDetails().errorMessage());
        System.exit(1);
    }
}
```

------
#### [ JavaScript ]

```
// Import required AWS SDK clients and commands for Node.js
import {PutDataProtectionPolicyCommand, GetDataProtectionPolicyCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const putParams = { ResourceArn: "TOPIC_ARN", DataProtectionPolicy: "DATA_PROTECTION_POLICY" };

const runPut = async () => {
  try {
    const data = await snsClient.send(new PutDataProtectionPolicyCommand(putParams));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
runPut();

// Set the parameters
const getParams = { ResourceArn: "TOPIC_ARN" };

const runGet = async () => {
  try {
    const data = await snsClient.send(new GetDataProtectionPolicyCommand(getParams));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
runGet();
```

------