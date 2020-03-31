# API errors for Amazon SNS mobile push<a name="mobile-push-api-error"></a>

Errors that are returned by the Amazon SNS APIs for mobile push are listed in the following table\. For more information about the Amazon SNS APIs for mobile push, see [Using Amazon SNS mobile push APIs](mobile-push-api.md)\.


| Error | Description | HTTPS status code | API Action | 
| --- | --- | --- | --- | 
| Application Name is null string | The required application name is set to null\. | 400 | `CreatePlatformApplication` | 
| Platform Name is null string | The required platform name is set to null\. | 400 | `CreatePlatformApplication` | 
| Platform Name is invalid | An invalid or out\-of\-range value was supplied for the platform name\. | 400 | `CreatePlatformApplication` | 
| APNs — Principal is not a valid certificate | An invalid certificate was supplied for the APNs principal, which is the SSL certificate\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `CreatePlatformApplication` | 
| APNs — Principal is a valid cert but not in a \.pem format | A valid certificate that is not in the \.pem format was supplied for the APNs principal, which is the SSL certificate\. | 400 | `CreatePlatformApplication` | 
| APNs — Prinicipal is an expired certificate | An expired certificate was supplied for the APNs principal, which is the SSL certificate\. | 400 | `CreatePlatformApplication` | 
| APNs — Principal is not an Apple issued certificate | A non\-Apple issued certificate was supplied for the APNs principal, which is the SSL certificate\. | 400 | `CreatePlatformApplication` | 
| APNs — Principal is not provided | The APNs principal, which is the SSL certificate, was not provided\. | 400 | `CreatePlatformApplication` | 
| APNs — Credential is not provided | The APNs credential, which is the private key, was not provided\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `CreatePlatformApplication` | 
| APNs — Credential are not in a valid \.pem format | The APNs credential, which is the private key, is not in a valid \.pem format\. | 400 | `CreatePlatformApplication` | 
| FCM — serverAPIKey is not provided | The FCM credential, which is the API key, was not provided\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `CreatePlatformApplication` | 
| FCM — serverAPIKey is empty | The FCM credential, which is the API key, is empty\. | 400 | `CreatePlatformApplication` | 
| FCM — serverAPIKey is a null string | The FCM credential, which is the API key, is null\. | 400 | `CreatePlatformApplication` | 
| FCM — serverAPIKey is invalid | The FCM credential, which is the API key, is invalid\. | 400 | `CreatePlatformApplication` | 
| ADM — clientsecret is not provided | The required client secret is not provided\. | 400 | `CreatePlatformApplication` | 
| ADM — clientsecret is a null string | The required string for the client secret is null\. | 400 | `CreatePlatformApplication` | 
| ADM — client\_secret is empty string | The required string for the client secret is empty\. | 400 | `CreatePlatformApplication` | 
| ADM — client\_secret is not valid | The required string for the client secret is not valid\. | 400 | `CreatePlatformApplication` | 
| ADM — client\_id is empty string | The required string for the client ID is empty\. | 400 | `CreatePlatformApplication` | 
| ADM — clientId is not provided | The required string for the client ID is not provided\. | 400 | `CreatePlatformApplication` | 
| ADM — clientid is a null string | The required string for the client ID is null\. | 400 | `CreatePlatformApplication` | 
| ADM — client\_id is not valid | The required string for the client ID is not valid\. | 400 | `CreatePlatformApplication` | 
| EventEndpointCreated has invalid ARN format | EventEndpointCreated has invalid ARN format\. | 400 | `CreatePlatformApplication` | 
| EventEndpointDeleted has invalid ARN format | EventEndpointDeleted has invalid ARN format\. | 400 | `CreatePlatformApplication` | 
| EventEndpointUpdated has invalid ARN format | EventEndpointUpdated has invalid ARN format\. | 400 | `CreatePlatformApplication` | 
| EventDeliveryAttemptFailure has invalid ARN format | EventDeliveryAttemptFailure has invalid ARN format\. | 400 | `CreatePlatformApplication` | 
| EventDeliveryFailure has invalid ARN format | EventDeliveryFailure has invalid ARN format\. | 400 | `CreatePlatformApplication` | 
| EventEndpointCreated is not an existing Topic | EventEndpointCreated is not an existing topic\. | 400 | `CreatePlatformApplication` | 
| EventEndpointDeleted is not an existing Topic | EventEndpointDeleted is not an existing topic\. | 400 | `CreatePlatformApplication` | 
| EventEndpointUpdated is not an existing Topic | EventEndpointUpdated is not an existing topic\. | 400 | `CreatePlatformApplication` | 
| EventDeliveryAttemptFailure is not an existing Topic | EventDeliveryAttemptFailure is not an existing topic\. | 400 | `CreatePlatformApplication` | 
| EventDeliveryFailure is not an existing Topic | EventDeliveryFailure is not an existing topic\. | 400 | `CreatePlatformApplication` | 
| Platform ARN is invalid | Platform ARN is invalid\. | 400 | `SetPlatformAttributes` | 
| Platform ARN is valid but does not belong to the user | Platform ARN is valid but does not belong to the user\. | 400 | `SetPlatformAttributes` | 
| APNs — Principal is not a valid certificate | An invalid certificate was supplied for the APNs principal, which is the SSL certificate\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `SetPlatformAttributes` | 
| APNs — Principal is a valid cert but not in a \.pem format | A valid certificate that is not in the \.pem format was supplied for the APNs principal, which is the SSL certificate\. | 400 | `SetPlatformAttributes` | 
| APNs — Prinicipal is an expired certificate | An expired certificate was supplied for the APNs principal, which is the SSL certificate\. | 400 | `SetPlatformAttributes` | 
| APNs — Principal is not an Apple issued certificate | A non\-Apple issued certificate was supplied for the APNs principal, which is the SSL certificate\. | 400 | `SetPlatformAttributes` | 
| APNs — Principal is not provided | The APNs principal, which is the SSL certificate, was not provided\. | 400 | `SetPlatformAttributes` | 
| APNs — Credential is not provided | The APNs credential, which is the private key, was not provided\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `SetPlatformAttributes` | 
| APNs — Credential are not in a valid \.pem format | The APNs credential, which is the private key, is not in a valid \.pem format\. | 400 | `SetPlatformAttributes` | 
| FCM — serverAPIKey is not provided | The FCM credential, which is the API key, was not provided\. For more information, see [CreatePlatformApplication](https://docs.aws.amazon.com/sns/latest/api/API_CreatePlatformApplication.html) in the Amazon Simple Notification Service API Reference\. | 400 | `SetPlatformAttributes` | 
| FCM — serverAPIKey is a null string | The FCM credential, which is the API key, is null\. | 400 | `SetPlatformAttributes` | 
| ADM — clientId is not provided | The required string for the client ID is not provided\. | 400 | `SetPlatformAttributes` | 
| ADM — clientid is a null string | The required string for the client ID is null\. | 400 | `SetPlatformAttributes` | 
| ADM — clientsecret is not provided | The required client secret is not provided\. | 400 | `SetPlatformAttributes` | 
| ADM — clientsecret is a null string | The required string for the client secret is null\. | 400 | `SetPlatformAttributes` | 
| EventEndpointUpdated has invalid ARN format | EventEndpointUpdated has invalid ARN format\. | 400 | `SetPlatformAttributes` | 
| EventEndpointDeleted has invalid ARN format | EventEndpointDeleted has invalid ARN format\. | 400 | `SetPlatformAttributes` | 
| EventEndpointUpdated has invalid ARN format | EventEndpointUpdated has invalid ARN format\. | 400 | `SetPlatformAttributes` | 
| EventDeliveryAttemptFailure has invalid ARN format | EventDeliveryAttemptFailure has invalid ARN format\. | 400 | `SetPlatformAttributes` | 
| EventDeliveryFailure has invalid ARN format | EventDeliveryFailure has invalid ARN format\. | 400 | `SetPlatformAttributes` | 
| EventEndpointCreated is not an existing Topic | EventEndpointCreated is not an existing topic\. | 400 | `SetPlatformAttributes` | 
| EventEndpointDeleted is not an existing Topic | EventEndpointDeleted is not an existing topic\. | 400 | `SetPlatformAttributes` | 
| EventEndpointUpdated is not an existing Topic | EventEndpointUpdated is not an existing topic\. | 400 | `SetPlatformAttributes` | 
| EventDeliveryAttemptFailure is not an existing Topic | EventDeliveryAttemptFailure is not an existing topic\. | 400 | `SetPlatformAttributes` | 
| EventDeliveryFailure is not an existing Topic | EventDeliveryFailure is not an existing topic\. | 400 | `SetPlatformAttributes` | 
| Platform ARN is invalid | The platform ARN is invalid\. | 400 | `GetPlatformApplicationAttributes` | 
| Platform ARN is valid but does not belong to the user | The platform ARN is valid, but does not belong to the user\. | 403 | `GetPlatformApplicationAttributes` | 
| Token specified is invalid | The specified token is invalid\. | 400 | `ListPlatformApplications` | 
| Platform ARN is invalid | The platform ARN is invalid\. | 400 | `ListEndpointsByPlatformApplication` | 
| Platform ARN is valid but does not belong to the user | The platform ARN is valid, but does not belong to the user\. | 404 | `ListEndpointsByPlatformApplication` | 
| Token specified is invalid | The specified token is invalid\. | 400 | `ListEndpointsByPlatformApplication` | 
| Platform ARN is invalid | The platform ARN is invalid\. | 400 | `DeletePlatformApplication` | 
| Platform ARN is valid but does not belong to the user | The platform ARN is valid, but does not belong to the user\. | 403 | `DeletePlatformApplication` | 
| Platform ARN is invalid | The platform ARN is invalid\. | 400 | `CreatePlatformEndpoint` | 
| Platform ARN is valid but does not belong to the user | The platform ARN is valid, but does not belong to the user\. | 404 | `CreatePlatformEndpoint` | 
| Token is not specified | The token is not specified\. | 400 | `CreatePlatformEndpoint` | 
| Token is not of correct length | The token is not the correct length\. | 400 | `CreatePlatformEndpoint` | 
| Customer User data is too large | The customer user data cannot be more than 2048 bytes long in UTF\-8 encoding\. | 400 | `CreatePlatformEndpoint` | 
| Endpoint ARN is invalid | The endpoint ARN is invalid\. | 400 | `DeleteEndpoint` | 
| Endpoint ARN is valid but does not belong to the user | The endpoint ARN is valid, but does not belong to the user\. | 403 | `DeleteEndpoint` | 
| Endpoint ARN is invalid | The endpoint ARN is invalid\. | 400 | `SetEndpointAttributes` | 
| Endpoint ARN is valid but does not belong to the user | The endpoint ARN is valid, but does not belong to the user\. | 403 | `SetEndpointAttributes` | 
| Token is not specified | The token is not specified\. | 400 | `SetEndpointAttributes` | 
| Token is not of correct length | The token is not the correct length\. | 400 | `SetEndpointAttributes` | 
| Customer User data is too large | The customer user data cannot be more than 2048 bytes long in UTF\-8 encoding\. | 400 | `SetEndpointAttributes` | 
| Endpoint ARN is invalid | The endpoint ARN is invalid\. | 400 | `GetEndpointAttributes` | 
| Endpoint ARN is valid but does not belong to the user | The endpoint ARN is valid, but does not belong to the user\. | 403 | `GetEndpointAttributes` | 
| Target ARN is invalid | The target ARN is invalid\. | 400 | `Publish` | 
| Target ARN is valid but does not belong to the user | The target ARN is valid, but does not belong to the user\. | 403 | `Publish` | 
| Message format is invalid | The message format is invalid\. | 400 | `Publish` | 
| Message size is larger than supported by protocol/end\-service | The message size is larger than supported by the protocol/end\-service\. | 400 | `Publish` | 