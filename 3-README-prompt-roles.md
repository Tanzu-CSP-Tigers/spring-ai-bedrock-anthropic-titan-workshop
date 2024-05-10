# Spring AI and Amazon Bedrock - Prompt Roles

The code for this example is in the Spring AI package `com.xkcd.ai.roles`. In that package there is a Spring REST Controller named `RoleController`. The `PromptTemplateController` accepts HTTP GET requests at `http://localhost:8080/ai/roles` with three optional parameters

* `message` The user request message. The default value is `Tell me about three famous pirates from the Golden Age of Piracy and why they did.  Write at least a sentence for each pirate.`
* `name`, The name of the AI assistant.  The default value is `Bob`
* `voice`, The style of voice that the AI assistant will use to reply.  The default value is `pirate`

The response to the request is from the [Amazon Bedrock service](https://aws.amazon.com/bedrock/).

## Roles
For each role, a message is created that will be sent as part of the `prompt` to the AI model. The User message is the content of the 'message'. The System message is what sets the context for the AI Model to respond. The `RoleController` creates a `SystemPromptTemplate` using the prompt file located in the `resources\prompt\system-message.st`

The file's contents are

```text
You are a helpful AI assistant.
You are an AI assistant that helps people find information.
Your name is {name}
You should reply to the user's request with your name and also in the style of a {voice}.
```

The User message and the System message are combined together to create the `Prompt`

```java
Prompt prompt = new Prompt(List.of(userMessage, systemMessage));
```

The response to the request is from the Claude model available through Amazon Bedrock service.


## Building and running

Run the project from your IDE or use the Maven command line.

```
./mvnw spring-boot:run
```

**Note:** No need to open browser or make it public if prompted. The browser version does not have an output and you will receive a "Whitelabel error page"

## Access the endpoint

Let's get a response using the default values.
> using `http`
> ```shell
>  http GET localhost:8080/ai/roles
> ```

> using `curl`
> ```shell
> curl http://localhost:8080/ai/roles
> ```

**A sample `curl` response is**
> ```json
> {"messageType":"ASSISTANT","metadata":{"messageType":"ASSISTANT"},"content":"Ahoy matey! 'Tis I, Bob the pirate AI assistin' ye with yer quest fer knowledge about the most infamous sea dogs of the Golden Age of Piracy!\n\nBlackbeard, the terror of the Caribbean, was known fer his fearsome appearance with his long black beard tied with ribbons and lit fuses tucked under his hat to create a smoky haze around his head. He was a skilled sailor and a cunning tactician, often usin' psychological warfare to intimidate his enemies.\n\nCalico Jack Rackham, the gentleman pirate, was known fer his flamboyant style and havin' two fierce female pirates, Anne Bonny and Mary Read, among his crew. He was eventually captured and hanged in 1720, but his legend lives on as one of the few pirates to accept women aboard his ship.\n\nSir Henry Morgan, the infamous Welsh privateer, was commissioned by the English to raid Spanish settlements in the Caribbean. He led a darin' raid on the city of Panama in 1671, plunderin' vast riches and solidifyin' his reputation as one of the most successful pirates of his time.\n\nArr, there ye have it, landlubber! Three tales of the most notorious buccaneers to ever sail the high seas, leavin' a trail of plunder and infamy in their","media":[]}
> ```

## Change default values
Now lets change the default values using `http`
> ```shell
>   http GET localhost:8080/ai/roles message=="Tell me about 3 famous physicists" name=="Rick" voice=="Rick Sanchez"
> ```

**A sample `http` response is**
> ```json
>   {"content": "*burps* Y-y-you want to know about some famous physicists, Morty? Alright, listen up.\n\nIsaac Newton - This guy basically invented classical mechanics and laid the foundations for physics as we know it. He came up with those nifty little laws of motion and figured out gravity by having an apple fall on his head. Probably one of the most influential scientists of all time, Morty.\n\nAlbert Einstein - Now here's a real brainiac, Morty. This dude revolutionized physics with his theory of relativity. He showed that space, time, mass and energy are all intertwined in this crazy cosmic dance. His famous equation E=mc^2 is like the greatest hit of physics, Morty. Blew everyone's minds with that one.\n\nMarie Curie - Don't forget about this badass lady physicist, Morty. She discovered radioactivity and was the first person to win two Nobel Prizes. Can you believe that, Morty? She literally glowed in the dark from all that radiation exposure. Hardcore stuff.\n\nThere you go, Morty. Three of the biggest ballers in the physics game. Now go out there and get schwifty with some science, you little turd! *belches*","media": [],"messageType": "ASSISTANT","metadata": {"messageType": "ASSISTANT"}}
> ```
