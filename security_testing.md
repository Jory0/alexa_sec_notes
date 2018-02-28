# Alexa Security Testing

There are separate requirements when you are hosting using AWS Lambda vs. your own endpoint.

## 1.1 Skills Hosted as Lambda Functions

## 1.2 Skills Hosted as Web Services on Your Own Endpoint

- The web service __must__ present a _valid, trusted certifcate_ when the connection is established and must possess the corresponding private key.
- Amazon only trusts certs that have been signed by an Amazon-approved certificate authority.
  - [The list of certificates listed here](https://ccadb-public.secure.force.com/mozilla/IncludedCACertificateReport)
  - Also letsEncrypt
- Self-signed certifications cannot be used for published skills
- Verify the incoming requests are sent by the Alexa service.
  - See [Hosting a Custom Skill as a Web Service](https://developer.amazon.com/docs/custom-skills/host-a-custom-skill-as-a-web-service.html)
  - MAKE SURE THE `disableRequestSignature` flag in the `SpeechletServlet` IS SET TO FALSE IN BEFORE PUSHED TO PROD!!
- Verify the incoming requests are intended for your service.

## 1.3 Skills with Account Linking

These steps are only necessary if the skill connects the identity of the end user with a user in another system (_account linking_). Verify that the skill follows ALL the instructions defined in [Linking an Alexa User in Your System](https://developer.amazon.com/docs/custom-skills/link-an-alexa-user-with-a-user-in-your-system.html). When submitting the skill, you must provide a valid set of account credentials with your testing instructions so Amazon's cert team can verify the account linking and functionality of the skill.

The skill must also pass the following criteria:

- Must use Amazon's account linking feature by redirecting the user to a login page or landing page when enabling the skill with the Alexa app.
- The skill's _privacy policy and terms of use_ links must open to a valid web page.
- If we are the owner of the credential system, we must pass the following criteria:
  - Own the domain of the login page.
  - The login page must be served over HTTPS.
- If not the owner of the credential system, pass the following criteria:
  - Must own the landing page used when enabling the skill. The page must clearly communicate which third-party account are needed to link the account to the skill.
  - The landing page must direct the user to the domain login page owned by the OAuth providers and must be served over HTTPS.
  - **You may not directly handle, store, or transmit credentials on behalf of the user.**
- If you are using Login with Amazon, your skill must pass the following criteria:
  - The login page URL must be from `amazon.com` and the page must be served over HTTPS.
  - The login page must clearly communicate which third-party accounts are needed to link the account to the skill.
  - You must clearly state the customer information your skill is collecting and using. This can be directly on the login page or in your privacy policy.

## 1.4 Skills that Allow Unlocking or Disarming

If the skill lets the user unlock or disarm a device, **you must require the user to speak a PIN of _at least_ four digits** before executing the unlock / disarm action.

- Remind the user to reset their PIN every 60 days.
- The user must be instructed to set a PIN in order to use the disarm/arm features.
- Every request must require a PIN number.
- After three consecutive failed PIN attempts, the skill instructs the user to reset their PIN.

## 2.5 Skills for Booking Reservations
