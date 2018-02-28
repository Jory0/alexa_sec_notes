# Lambda Alexa Skill Security

Using a Lambda function eliminates some of the complexity around setting up and managing your own endpoint:

- You do not need to administer or manage any of the compute resources for your service.
- You do not need an SSL certificate
- You do not need to verify that requests are coming from the Alexa service yourself. Access to execute your function is controlled by permissions within AWS instead.
- Alexa encrypts its communications with Lambda utilizing TLS. See also [AWS security best practices](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Best_Practices.pdf)

Lambda functions must adhere to the same service interface and handle the three types of requests sent by Alexa. [Requests By Alexa](https://developer.amazon.com/docs/custom-skills/handle-requests-sent-by-alexa.html). For details on the JSON interface, see [JSON Interface Reference for Custom Skills](https://developer.amazon.com/docs/custom-skills/request-and-response-json-reference.html)

You must configure at least one _trigger_ for your function to grant Alexa the necessary _invocation permissions_ for your function.

When you add the Alexa Skills Kit as a trigger, **you must also enable _skill ID verification_ for the function and provide the _skill ID_ (also called the _application ID_) for your skill.** this means that your function can be invoked only if the skill ID  in the ASK request matches the skill ID configured in the trigger.

**When using skill ID verification, you do not need to include code to verify that the request is intended for your service.**

> ### Multiple Skills Calling the Same Function
> You can configure multiple triggers if you want to invoke the same Lambda function from multiple skills. Each trigger must be configured with a unique skill ID
