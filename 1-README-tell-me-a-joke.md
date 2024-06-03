# Spring AI and Amazon Bedrock - Tell me a joke
The code for this example is in the Spring AI package `com.xkcd.ai.helloworld`. In that package there is a Spring REST Controller named `SimpleAiController`. The `SimpleAiController` accepts HTTP GET requests at `http://localhost:8080/ai/simple`

There is optional `message` parameter whose default value is "Tell me a joke". The response to the request is from the AWS Bedrock Service. The `SimpleAiController` accepts HTTP GET requests at `http://localhost:8080/ai/simple`

There is optional `message` parameter whose default value is "Tell me a joke". The response to the request is from the AWS Bedrock Service.

## Building and running

Run the project from your IDE or use the Maven command line
```
./mvnw spring-boot:run
```
**NOTE:** No need to open browser or make it public if prompted. The browser version does not have an output and you will receive a "Whitelabel error page"

## Access the endpoint
Start a new Terminal window,if needed, to execute the commands below.

To get a response to the default request of "Tell me a joke" using the `http` or 'curl' command line utility
> using **`http`**
> ```shell
> http http://localhost:8080/ai/simple
> ```

> using **`curl`**
> ```shell
> curl http://localhost:8080/ai/simple
> ```

**A sample response is**

```text
Why don't scientists trust atoms?

Because they make up everything!
```

## Request parameter
Now using the `text` request parameter:

> using **`http`**
> ```shell
> http GET localhost:8080/ai/simple message=='Tell me a joke about a cow.' 
> ```

> using **`curl`**
> ```shell
> curl --get  --data-urlencode 'message=Tell me a joke about a cow.' http://localhost:8080/ai/simple 
> ```

**A sample response is**
```text
Why did the cow go to outer space? To see the moooon!
```
