# Spring AI Workshop for AWS Bedrock

## Introduction
This workshop has been adapted from the excellent [Spring AI workshop]([https://github.com/Azure-Samples/spring-ai-azure-workshop/tree/main](https://github.com/spring-projects/spring-ai)) for AWS Bedrock.  Unlike alternatives, AWS has chosen to host & provide multiple models, rather than throw in entirely on their own model (Titan).  The models selected for this workshop are `Anthropic's 3` for chat and AWS's proprietary `Titan` for embeddings.

**NOTE:** Many users will have experience doing AI workshops using python tools like *langchain* and *streamlit*.  While that provides a nice GUI for testing, Spring is designed for enterprise applications and is almost always part of a larger application. Because Spring in general is a tool for developers, the entire workshop will be delivered through the command line terminal.  While it is definitely possible to run command by posting commands such as "http://localhost:8080/ai/simple" in a browser, it is not really an optimal experience.  

For best experience, we encourage user to deploy the *.devcontainer*, which has all necessary tools such as `java`, `maven`, `http`, and `curl` installed. Launching a [CodeSpace]("https://github.com/features/codespaces") from GitHub will open the *devcontainer* and bring you into the *VsCode* tool. In this case, we recommend running `./mvnw clean install`before starting the first exercise to download all dependencies and prep the application. Also, it is not necessary to run `./mvnw spring-boot:run` for every exercise if the app is still running.  

Of course, users are welcome to download this repo and run it locally using their own tools as well.  We also encourage users to visit [Spring Initializer](https://start.spring.io") to recreate the code on their own.

## Prerequisites

Before you begin make sure to set the following environment variables.

```shell
export AWS_ACCESS_KEY_ID=<INSERT ID HERE>
export AWS_SECRET_ACCESS_KEY=<INSERT KEY HERE>
export AWS_SESSION_TOKEN=<INSERT TOKEN HERE>  \\optional for some users
```
NOTE: the [Spring AI docs]("https://docs.spring.io/spring-ai/reference/index.html) will tell you to enter these values in the *application.properties* file.  
I found I had issues with the utility used in-house to access AWS, so I leave them out of *application.properties*.   However, you *must* set the region in the *application.properties* file.

### Enable Bedrock AI Model Access

The configuration assumes you have already enabled the required models inside your AWS Bedrock account.  You can enable model access at the model access tab on the bottom-left of teh main page.  e.g. [here](https://us-east-1.console.aws.amazon.com/bedrock/home?region=us-east-1#/modelaccess).   This workshop requires access to `Anthropic` for chat feature and `Titan` only for embeddings.   At the time of this writing `Anthropic` does require a use case justification.


For the workshop, the configuration in `application.properties` should contain the following

```shell
spring.ai.bedrock.aws.region=<INSERT AWS REGION>

spring.ai.bedrock.anthropic3.chat.enabled=true

spring.ai.bedrock.titan.embedding.enabled=true
spring.ai.bedrock.titan.embedding.model=amazon.titan-embed-text-v1
spring.ai.bedrock.titan.embedding.input-type=TEXT
```

## Workshop Overview

The workshop consists of six examples, each with a dedicated `README` file.  Note that there is a description of how Spring, and the goal is to instruct user on how SpringAI functions.   The code is essentially the same with every AI model.  

All six workshop examples are organized into individual Java packages within this project. In each package, you'll find a Spring @RestController class that serves as the entry point for showcasing the discussed functionality.

To interact with the @RestController, you will be using the `http` utility as a user-friendly alternative to `curl`.

Detailed instructions and exercises for each example can be found in their respective README files:

* 1-README-tell-me-a-joke.md 
* 2-README-prompt-templating.md 
* 3-README-prompt-roles.md 
* 4-README-output-parser.md 
* 5-README-stuff-prompt.md 
* 6-README-retrieval-augmented-generation.md

These guides will walk you through the workshop exercises.

`NOTE:` before running the 
