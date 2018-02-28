# Alexa Security

First, in order to better describe how to write secure Alexa skills we must go over the how a skill is developed.

Before you begin writing your first skill you must decide what  _type_ of skill you are going to create.
You may choose from a list of pre-defined skills or create an entirely custom skill. The security requirements are different for each type.

- Smart Home Skill API
  - This is a skill for interacting with IoT devices.

- Video Skill API
  - This is a skill for interacting with cloud-based video services.

- Flash Briefing Skill API
  - Defines the words users say to invoke the flash briefing or news requests and the format of the content for the customer

- Custom
  - Whatever you want it to be!

**Invocation Name**
  - This is what lets the Alexa service know which skill the user is wanting to use.
**Intents**
  - Describe the structure of intents you see in the code editor.
    - Slots, name, & samples
  Essentially, intents are what the users intends to do with the skill. An intent is triggered by a list of selected phrases that are defined by you.
    - ex. I am driving from Gainesville to Seattle.
  Slots are optional arguments within a phrase that is used when processing the intent.
    - ex. I am `{modeOfTransport} from {fromCity} to {toCity}`
**Writing Lambda Functions**
  - This is the actual functionality of the skill that the user is using.
**Writing Your Own Endpoints**
  - Same as the lambda function, but you are serving your own endpoint instead of integrating AWS.

1. Create the skill
  - Choose type
2. Create an intent
  - What will the user need to say in order to trigger this intent?
3. Create slots for intent (optional)
  - If your lambda function requires arguments for full functionality, add slots by wrapping certain words of the phrase in `{`
4. Choose or create the appropriate slot type for your slots.
  - Some Amazon provided slots will change the format for QOL changes.
5. Determine if the slot value is required in order to fulfill the request.
  - Write dialogue and utterances uses in conversation to elicit the slot.
6. For each required slot, determine whether not the user must explicitly confirm the action before the skill completes the request.
7. For the entire intent, determine whether the user must explicitly confirm the action before the skill completes the request.

If we are handling sensitive data with the skill, we need to require the user to confirm both the slots where the sensitive information is correct as well as well confirm the entire intent.
  - Do not have Alexa repeat sensitive data.
  - Provide MFA by either sending a text message or an email to the user that they must confirm.
    - Send them a temporary code that they must utter to the skill in order to continue.
    - This is an area of concern because a malicious user with access to the device could spam the owner of the device with text messages.
    - Asking for a PIN number is useless because any member of the household, person outside, or anyone with ears can listen in on the PIN number and still perform the actions of the skill.
