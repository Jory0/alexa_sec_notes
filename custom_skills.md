# Components of a Custom Skill

You will need the following:

- A set of _intents_ that represent actions that users can do with your skill. These intents represent the core functionality for your skill.
- A set of _sample utterances_ that specify the words and phrases users can say to invoke those intents. You may map these utterances to your intents. This mapping forms the _interaction model_ for the skill.
- An _invocation name_ that identifies the skill. The user includes this name when initiating a conversation with your skill.
- A _cloud-based_ service that accepts these intents as structured requests and acts upon them.

Speaking this to an Alexa-enabled device does the following:

The user's speech is streamed to the Alexa service in the cloud.
Alexa recognizes that this request represents the OneshotTideIntent intent for the "Tide Pooler" skill.
Alexa structures this information into a request (specifically an IntentRequest in this example) and sends this request to the service defined for Tide Pooler. The request includes the value "seattle" as the "City".
The Tide Pooler service gets the request and takes an appropriate action (looking up tide information for the current date in Seattle from http://tidesandcurrents.noaa.gov/).
Tide Pooler sends the Alexa service a structured response with the text to speak to the user.
The Alexa-enabled device speaks the response back to the user:

` Tide Pooler: Today in Seattle, the first high tide will be around 1:42 in the morning, and will peak at about 10 feet…`


