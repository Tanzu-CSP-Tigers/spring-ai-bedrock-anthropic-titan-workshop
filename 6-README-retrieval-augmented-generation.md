# Spring AI and Amazon Bedrock - Retrieval Augmented Generation
The code for this example is in the Spring AI package `com.xkcd.ai.rag`. In that package there is a Spring REST Controller named `RagController`. The `RagController` accepts HTTP GET requests at `http://localhost:8080/ai/rag` with one optional parameter

* `message` The user question that can be answered using the data provided to the model.
 
The **defaultValue** is `What bike is good for city commuting?`.

## Code Walkthrough
The `RagController` delegates to the `RagService`. The `RagService` uses two Spring Resources.
* `classpath:/data/bikes.json` contains the catalog information about bikes.  This is the set of data that we are bringing to the AI model
* `classpath:/prompts/system-qa.st` contains the system prompt. Information about bikes that are similar to the use question will be 'stuffed' into the System prompt.

The steps of processing are

### Load the documents
```java
        JsonReader jsonReader = new JsonReader(bikesResource,
                "name", "price", "shortDescription", "description");
        List<Document> documents = jsonReader.get();
```

Create embeddings for the documents.  This calls the Amazon Bedrock embedding endpoint.

```java
        VectorStore vectorStore = new SimpleVectorStore(embeddingClient);
        vectorStore.add(documents);
```
### Find documents similar to the query
```java
        List<Document> similarDocuments = vectorStore.similaritySearch(message);
```

### Create a Prompt
The `prompt` is created from a system message and a user message.  The system message contains the similar documents retrieved from the `VectorStore`.  The user message is the user's input to the `RagController` request parameter `message`.

```java
        Message systemMessage = getSystemMessage(similarDocuments);
        UserMessage userMessage = new UserMessage(message);
        Prompt prompt = new Prompt(List.of(systemMessage, userMessage));
```

### Get the response
```java
        ChatResponse chatResponse = chatClient.call(prompt);
```

The response to the request is from the Amazon Bedrock service.

## Building and running
Run the project from your IDE or use the Maven command line
```
./mvnw spring-boot:run
```
**NOTE:** No need to open browser or make it public if prompted. The site does not have an output if opened in a browser and you will receive a "Whitelabel error page"

## Access the endpoint
To get a response to the default request of "What bike is good for city commuting?"

> using `http`
> ```shell
> http http://localhost:8080/ai/rag
> ```

> using `curl`
> ```shell
> curl http://localhost:8080/ai/rag
> ```

**A sample response is**
```json
{
  "info": {},
  "text": "Both the SwiftRide Hybrid and the AgileEon 9X are good options for city commuting, as they are designed for daily commutes and recreational rides. They both have efficient electric assist, integrated lights, and components that provide a comfortable and reliable cycling experience. Ultimately, the choice depends on your personal preferences and needs."
}
```

Now using the `message` request parameter to ask about a specific bike.
```shell
$  http GET localhost:8080/ai/rag message=="Tell me some details about the SwiftRide Hybrid"
```

**A sample response is**
```json
{
    "info": {},
    "text": "The SwiftRide Hybrid is a versatile and efficient bike designed for riders who want a smooth and enjoyable ride on various terrains. It features a lightweight and durable aluminum frame, a powerful electric motor that offers a speedy assist, a removable and fully-integrated 500Wh battery, a 10-speed Shimano drivetrain, hydraulic disc brakes for precise stopping power, wide puncture-resistant tires for stability, and integrated lights for enhanced visibility. The bike is priced at $3999.99."
}
```

