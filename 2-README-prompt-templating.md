#  Spring AI and Amazon Bedrock - Prompt Templating 

The code for this example is in the Spring AI package `com.xkcd.ai.prompttemplate`. In that package there is a Spring REST Controller named `PromptTemplateController`.

The `PromptTemplateController` shows how to use the StringTemplate Engine and the Spring AI `PromptTemplate` class. In the `resources\prompts` directory is the file `joke-prompt`. That file is loaded using the Spring `Resource` abstraction in the controller as shown below


```java
    @Value("classpath:/prompts/joke-prompt.st")
    private Resource jokeResource;
```

The files contents are

```text
Tell me a {adjective} joke about {topic}
```

The `PromptTemplateController` accepts HTTP GET requests at `http://localhost:8080/ai/prompt` with two optional parameters

* `adjective`, whose default value is `funny`
* `topic`, whose default value is `cows`

The response to the request is from the AWS Bedrock Service.

## Building and running

Run the project from your IDE or use the Maven command line
```
./mvnw spring-boot:run
```
**NOTE:** No need to open browser or make it public if prompted. The browser version does not have an output and you will receive a "Whitelabel error page"

## Access the endpoint
To get a response for a funny joke about a cow.

> using `http`
> ```shell
> http GET localhost:8080/ai/prompt adjective==funny topic==cow
> ```

> using `curl`
> ```shell
> curl --get  --data-urlencode 'adjective=funny' --data-urlencode 'topic=cow' http://localhost:8080/ai/prompt
> ```

**A sample `curl` response is**
> {"messageType":"ASSISTANT","metadata":{"messageType":"ASSISTANT"},"content":"Here's a funny cow joke for you:\n\nWhy did the cow cross the road?\n\nTo get to the udder side!","media":[]}


