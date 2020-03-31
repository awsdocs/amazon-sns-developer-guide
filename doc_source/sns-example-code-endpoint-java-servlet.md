# Example code for an Amazon SNS endpoint Java servlet<a name="sns-example-code-endpoint-java-servlet"></a>

**Important**  
The following code snippets help you understand a Java servlet that processes Amazon SNS HTTP POST requests\. You should make sure that any portions of these snippets are suitable for your purposes before implementing them in your production environment\. For example, in a production environment to help prevent spoofing attacks, you should verify that the identity of the received Amazon SNS messages is from Amazon SNS\. You can do this by checking that the DNS Name value \(*DNS Name=sns\.us\-west\-2\.amazonaws\.com* in us\-west\-2; this will vary by Region\) for the *Subject Alternative Name* field, as presented in the Amazon SNS Certificate, is the same for the received Amazon SNS messages\. For more information about verifying server identity, see section [3\.1\. Server Identity](http://tools.ietf.org/search/rfc2818) in *RFC 2818*\. Also see [Verifying the signatures of Amazon SNS messages](sns-verify-signature-of-message.md)

The following method implements an example of a handler for HTTP POST requests from Amazon SNS in a Java servlet\.

```
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException, SecurityException {
    //Get the message type header.
    String messagetype = request.getHeader("x-amz-sns-message-type");
    //If message doesn't have the message type header, don't process it.
    if (messagetype == null) {
        return;
    }

    // Parse the JSON message in the message body
    // and hydrate a Message object with its contents
    // so that we have easy access to the name-value pairs
    // from the JSON message.
    Scanner scan = new Scanner(request.getInputStream());
    StringBuilder builder = new StringBuilder();
    while (scan.hasNextLine()) {
        builder.append(scan.nextLine());
    }
    Message msg = readMessageFromJson(builder.toString());

    // The signature is based on SignatureVersion 1.
    // If the sig version is something other than 1,
    // throw an exception.
    if (msg.getSignatureVersion().equals("1")) {
        // Check the signature and throw an exception if the signature verification fails.
        if (isMessageSignatureValid(msg)) {
            log.info(">>Signature verification succeeded");
        } else {
            log.info(">>Signature verification failed");
            throw new SecurityException("Signature verification failed.");
        }        
    } else {
        log.info(">>Unexpected signature version. Unable to verify signature.");
        throw new SecurityException("Unexpected signature version. Unable to verify signature.");
    }

    // Process the message based on type.
    if (messagetype.equals("Notification")) {
        //Do something with the Message and Subject.
        //Just log the subject (if it exists) and the message.
        String logMsgAndSubject = ">>Notification received from topic " + msg.getTopicArn();
        if (msg.getSubject() != null) {
            logMsgAndSubject += " Subject: " + msg.getSubject();
        }
        logMsgAndSubject += " Message: " + msg.getMessage();
        log.info(logMsgAndSubject);
    } else if (messagetype.equals("SubscriptionConfirmation")) {
        //You should make sure that this subscription is from the topic you expect. Compare topicARN to your list of topics
        //that you want to enable to add this endpoint as a subscription.

        //Confirm the subscription by going to the subscribeURL location
        //and capture the return value (XML message body as a string)
        Scanner sc = new Scanner(new URL(msg.getSubscribeURL()).openStream());
        StringBuilder sb = new StringBuilder();
        while (sc.hasNextLine()) {
            sb.append(sc.nextLine());
        }
        log.info(">>Subscription confirmation (" + msg.getSubscribeURL() + ") Return value: " + sb.toString());
        //Process the return value to ensure the endpoint is subscribed.
    } else if (messagetype.equals("UnsubscribeConfirmation")) {
        //Handle UnsubscribeConfirmation message.
        //For example, take action if unsubscribing should not have occurred.
        //You can read the SubscribeURL from this message and
        //re-subscribe the endpoint.
        log.info(">>Unsubscribe confirmation: " + msg.getMessage());
    } else {
        //Handle unknown message type.
        log.info(">>Unknown message type.");
    }
    log.info(">>Done processing message: " + msg.getMessageId());
}
```

The following example Java method creates a signature using information from a `Message` object that contains the data sent in the request body and verifies that signature against the original Base64\-encoded signature of the message, which is also read from the `Message` object\.

```
private static boolean isMessageSignatureValid(Message msg) {
    try {
        URL url = new URL(msg.getSigningCertURL());
        verifyMessageSignatureURL(msg, url);

        InputStream inStream = url.openStream();
        CertificateFactory cf = CertificateFactory.getInstance("X.509");
        X509Certificate cert = (X509Certificate) cf.generateCertificate(inStream);
        inStream.close();

        Signature sig = Signature.getInstance("SHA1withRSA");
        sig.initVerify(cert.getPublicKey());
        sig.update(getMessageBytesToSign(msg));
        return sig.verify(Base64.decodeBase64(msg.getSignature()));
    } catch (Exception e) {
        throw new SecurityException("Verify method failed.", e);
    }
}

private static void verifyMessageSignatureURL(Message msg, URL endpoint) {
    URI certUri = URI.create(msg.getSigningCertURL());

    if (!"https".equals(certUri.getScheme())) {
        throw new SecurityException("SigningCertURL was not using HTTPS: " + certUri.toString());
    }

    if (!endpoint.equals(certUri.getHost())) {
        throw new SecurityException(
                String.format("SigningCertUrl does not match expected endpoint. " +
                                "Expected %s but received endpoint was %s.",
                        endpoint, certUri.getHost()));

    }
}
```

The following example Java methods work together to create the string to sign for an Amazon SNS message\. The `getMessageBytesToSign` method calls the appropriate string\-to\-sign method based on the message type and runs the string to sign as a byte array\. The `buildNotificationStringToSign` and `buildSubscriptionStringToSign` methods create the string to sign based on the formats described in [Verifying the signatures of Amazon SNS messages](sns-verify-signature-of-message.md)\.

```
private static byte [] getMessageBytesToSign (Message msg) {
    byte [] bytesToSign = null;
    if (msg.getType().equals("Notification"))
        bytesToSign = buildNotificationStringToSign(msg).getBytes();
    else if (msg.getType().equals("SubscriptionConfirmation") || msg.getType().equals("UnsubscribeConfirmation"))
        bytesToSign = buildSubscriptionStringToSign(msg).getBytes();
    return bytesToSign;
}

//Build the string to sign for Notification messages.
public static String buildNotificationStringToSign(Message msg) {
    String stringToSign = null;

    //Build the string to sign from the values in the message.
    //Name and values separated by newline characters
    //The name value pairs are sorted by name 
    //in byte sort order.
    stringToSign = "Message\n";
    stringToSign += msg.getMessage() + "\n";
    stringToSign += "MessageId\n";
    stringToSign += msg.getMessageId() + "\n";
    if (msg.getSubject() != null) {
        stringToSign += "Subject\n";
        stringToSign += msg.getSubject() + "\n";
    }
    stringToSign += "Timestamp\n";
    stringToSign += msg.getTimestamp() + "\n";
    stringToSign += "TopicArn\n";
    stringToSign += msg.getTopicArn() + "\n";
    stringToSign += "Type\n";
    stringToSign += msg.getType() + "\n";
    return stringToSign;
}

//Build the string to sign for SubscriptionConfirmation 
//and UnsubscribeConfirmation messages.
public static String buildSubscriptionStringToSign(Message msg) {
    String stringToSign = null;
    //Build the string to sign from the values in the message.
    //Name and values separated by newline characters
    //The name value pairs are sorted by name 
    //in byte sort order.
    stringToSign = "Message\n";
    stringToSign += msg.getMessage() + "\n";
    stringToSign += "MessageId\n";
    stringToSign += msg.getMessageId() + "\n";
    stringToSign += "SubscribeURL\n";
    stringToSign += msg.getSubscribeURL() + "\n";
    stringToSign += "Timestamp\n";
    stringToSign += msg.getTimestamp() + "\n";
    stringToSign += "Token\n";
    stringToSign += msg.getToken() + "\n";
    stringToSign += "TopicArn\n";
    stringToSign += msg.getTopicArn() + "\n";
    stringToSign += "Type\n";
    stringToSign += msg.getType() + "\n";
    return stringToSign;
}
```