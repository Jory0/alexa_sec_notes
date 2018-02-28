# Custom Endpoint Security

These are the following requirements for your own custom web service:

1. The service must be internet-accessible
2. The service must adhere to the Alexa Skills Kit Interface
  - Alexa communicates with your service via a request-response mechanism using HTTP over SSL/TLS. When a user interacts with an Alexa skill, your service receives a POST request containing a JSON body. The request body contains the parameters necessary for the service to perform its logic and generate a JSON-formatted response.
  - Read more [here](https://developer.amazon.com/docs/custom-skills/request-and-response-json-reference.html)
3. The service **must** support HTTP over SSL/TLS, leveraging an Amazon-trusted certificate.
  - [List of CAs approved by Amazon](https://wiki.mozilla.org/CA:IncludedCAs)
4. The service must accept requests on port 443
5. The service must present a certificate with a subject alternate name that matches the domain name of the endpoint
6. The service must validate that incoming requests are coming from Alexa

## Verifying that the Request was Sent by Alexa

To protect your endpoint from potential attackers, your web service should verify that incoming requests were sent by Alexa. Any requests coming from other sources should be rejected.

There are two parts to validating incoming requests:

- Check the request signature to verify the authenticity of the request. Alexa signs all HTTPS requests.
  - This is _required_ for certifying your Alexa skill and making it available to Amazon users. Web services that accept unsigned requests or fail to verify the request signature are rejected.
  - The Java library does this verification in the `SpeechletServlet` class. If you do not use the Java library, then you must do the verification yourself.
  - If you use the Java library without using the `SpeechletServlet` class, you can use the `SpeechletRequestSignatureVerifier` class to do this.
- Check the request timestamp to ensure that the request is not an old request being sent as part of a "replay" attack.
  - This is _required_ for certifying your Alexa skill and making it available to Amazon users.
  - The Java library does this verification in the `SpeechletServlet`. You need to provide the tolerance to allow in _seconds_ in the system property `com.amazon.speech.speechlet.servlet.timestampTolerance`.
  - If you use the Java library without using the `SpeechletServlet` class, you can use the `TimeStampSpeehcletRequestVerifier` class to do this.

## Checking the Signature of the Request

Requests sent by Alexa provide the information you need to verify the signature in the HTTP headers:

- `SignatureCertChainUrl`
- `Signautre`
To validate the signature:
1. Verify the URL specified by the `SignatureCertChainUrl` header value on the request to ensure that it matches the format used by Amazon. **See verifying signature certificate URL**
2. Download the PEM-encoded X.509 certificate chain that Alexa used to sign the message as specified by the `SignatureCertChainUrl` header value on the request.
  - This chain is provided at runtime so that the certificate may be updated periodically, so your web service should be resilient to different URLs with different content.
3. This certificate chain is composed of, in order, (1) the Amazon signing certificate and (2) one or more additional certificates that create a chain of trust to a root CA certificate. To confirm the validity of the signing certificate, perform the following checks:
  - The signing certificate has not expired (examine both the `Not Before` and `Not After` dates)
  - The domain `echo-api.amazon.com` is present in the `Subject Alternative Names` (SANs) section of the signing certificate.
  - All certificates in the chain combine to create a chain of trust to a trusted root CA certificate.
4. Once you have determined that the signing certificate is valid, extract the public key from it.
5. Base64-decode the `Signature` header value on the request to obtain the encrypted signature.
6. Use the public key extracted from the signing certificate to decrypt the encrypted signature to produce the asserted hash value.
7. Generate a SHA-1 hash value from the full HTTPS request body to produce the derived hash value.
8. Compare the asserted hash value and the derived hash values to ensure that they match.
