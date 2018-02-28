# Build Skills with the Alexa Skills Kit (ASK)
 
There are types of Alexa Skills that can be built by using defined APIs. These APIs are:

1. [Smart Home Skill API](https://developer.amazon.com/docs/smarthome/understand-the-smart-home-skill-api.html)
  - This is for skills that interact with the user's "smart home".
2. [Video Skill API](https://developer.amazon.com/docs/video/understand-the-video-skill-api.html)
  - This is for interacting with cloud-enabled video services (play, find a show, change channel, etc)
3. [Flash Briefing Skill API](https://developer.amazon.com/docs/flashbriefing/understand-the-flash-briefing-skill-api.html)
  - Defines the words users say to invoke the flash briefing or news requests and the format of the content so that Alexa can provide it to the customer

## What Do I Build?

> You create a cloud-based service that handles the requests for the skill type and host it in the cloud. The Alexa service routes incoming requests to the appropriate service.

Different types of skills require different types of services:
For a _custom skill_ you code either an AWS Lambda function or a _web service_:

- AWS Lambda is a service that lets you run code in the cloud without managing servers.
  - Alexa sends user requests and the lambda function inspects the request, takes any necessary actions, and then send back a response.
- Alternatively, you can write a _web service_ and host it with any cloud hosting provider.
  - You must use HTTPS.
    - In this case, Alexa sends requests to your web service and your service takes any necessary actions and sends back a response.

## Understanding how Users Interact with Skills

When a user speaks to a device with Alexa, the speech is streamed to the Alexa service in the cloud. Alexa recognizes the speech, determines what the user wants, and then sends a structured request to the particular skill that can fulfill the user's request. All speech recognition and conversion is handled by Alexa in the cloud.

Every Alexa skill has an interaction model defining the words and phrases users can say to make the skill do what they want. The type of skill you build dictates the interaction model that Alexa uses to communicate with your users.

When interacting with a custom skill, the user must include a name identifying the skill (ex. "tide pooler")

### What is an Interaction Model?

This is analogous to a graphical user interface in a traditional app.

When users speak questions and make requests, Alexa uses the interaction model to interpret and translate the words into a specific request that can be handled by a particular skill. The request is then sent to the skill.

#### Interact with a Custom Skill
Note this phrase a user can speak:

User: `Alexa, get high tide for Seattle from Tide Pooler.`

"Tide Pooler" is the invocation name that identifies a particular skill. When invoking a custom skill, users must include this name.
"get high tide for Seattle" is a phrase in Tide Pooler's interaction model. This phrase is mapped to a specific intent supported by this skill.
Alexa uses this custom interaction model to create a structured representation of the request called an intent. Alexa sends the intent to the Tide Pooler skill. The skill can then look up tide information and send back a response.

## What Do You Need for a Custom Skill?

- An internet-accessible endpoint for hosting your cloud-based services
  - When using Lambda, you will need to write code using Node.js, Java, Python, or C#.
- A publicly accessible web site to host any images, audio files, or video files that you use in your skill.
  - If you are only using a skill icon, then you do not need to host any resources.
- Use a device or test using the service simulator.
