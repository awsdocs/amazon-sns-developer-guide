# Create a platform endpoint for Amazon SNS push notifications using an AWS SDK<a name="example_sns_CreatePlatformEndpoint_section"></a>

The following code example shows how to create a platform endpoint for Amazon SNS push notifications\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Java ]

**SDK for Java 2\.x**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/sns#readme)\. 
  

```
public class RegistrationExample {

    public static void main(String[] args) {

        final String usage = "\n" +
            "Usage: " +
            "    <token>\n\n" +
            "Where:\n" +
            "   token - The name of the FIFO topic. \n\n" +
            "   platformApplicationArn - The ARN value of platform application. You can get this value from the AWS Management Console. \n\n";

        if (args.length != 2) {
            System.out.println(usage);
            System.exit(1);
        }

        String token = args[0];
        String platformApplicationArn = args[1];
        SnsClient snsClient = SnsClient.builder()
            .region(Region.US_EAST_1)
            .credentialsProvider(ProfileCredentialsProvider.create())
            .build();

        createEndpoint(snsClient, token, platformApplicationArn);
    }

    public static void createEndpoint(SnsClient snsClient, String token, String platformApplicationArn){

        System.out.println("Creating platform endpoint with token " + token);

        try {
            CreatePlatformEndpointRequest endpointRequest = CreatePlatformEndpointRequest.builder()
                .token(token)
                .platformApplicationArn(platformApplicationArn)
                .build();

            CreatePlatformEndpointResponse response = snsClient.createPlatformEndpoint(endpointRequest);
            System.out.println("The ARN of the endpoint is " + response.endpointArn());
        } catch ( SnsException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);
        }
    }
}
```

------

For a complete list of AWS SDK developer guides and code examples, see [Using Amazon SNS with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.