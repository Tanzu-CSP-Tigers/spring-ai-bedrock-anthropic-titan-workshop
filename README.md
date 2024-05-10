# Spring AI Workshop for AWS Bedrock

## Introduction
This is an introductory workshop for Spring AI with AWS Bedrock LLMs.  Unlike vendors like Azure and Google, AWS has chosen to host multiple models rather than throw in entirely on their own model (Titan).  The model I selected for this workshop is `Anthropic3` for chat, and AWS's own `Titan` for embeddings.

> **NOTE:** Many users will have experience doing AI workshops using python tools like *langchain* and *streamlit*.  While that provides a nice GUI for testing, Spring is designed for enterprise applications and is almost always part of a larger application. Because [Spring](https://spring.io/) in general is a tool for developers, the entire workshop will be delivered through the command line terminal.  While it is definitely possible to run command by posting commands such as _ht<span>tp://localhost</span>:8080/ai/simple_ in a browser, it is not really an optimal experience.  

**For best experience,** we encourage user to deploy the *.devcontainer*, which has all necessary tools such as `java`, `maven`, `http`, and `curl` installed. Launching a [CodeSpace]("https://github.com/features/codespaces") from GitHub will open the *devcontainer* and bring you into the *VsCode* tool. In this case, we recommend running `./mvnw clean install`before starting the first exercise to download all dependencies and prep the application. Also, it is not necessary to run `./mvnw spring-boot:run` for every exercise if the app is still running.  

Users are welcome to download this repo and run it locally using their own tools as well.  We also encourage users to visit [Spring Initializer](https://start.spring.io") to recreate the code on their own.

## Prerequisites
Before you begin setup the prerequisites and follow least security permissions and [AWS best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html).

1/ Setup new IAM user if one is not already available with minimum of Command Line Interface (CLI) capabilities. 

2/ Attach permissions to the new user with permissions to execute Bedrock API calls. We found if you do not have a custom policy that PowerUserAccess works well. Note the AWS Access Key and Security Key IDs

3/ Set the environmental variables in the IDE terminal for the AWS Access Key and Security Key IDs 
```shell
export AWS_ACCESS_KEY_ID=<INSERT ID HERE>
export AWS_SECRET_ACCESS_KEY=<INSERT KEY HERE>
```
> **NOTE:** the [Spring AI documentation]("https://docs.spring.io/spring-ai/reference/index.html) will tell you to enter these values in the /src/main/resources/application.properties file. We found issues with the utility used in-house to access AWS, so I leave them out of application.properties opting for setting environmental variables in my console.

4/  However set the region in /src/main/resources/application.properties.  Example:  spring.ai.bedrock.aws.region=us-east-1

### Enable Bedrock AI Model Access

The configuration assumes you have already enabled the required models inside your AWS Bedrock account.  You can enable model access at the model access tab on the bottom-left of the main page.  e.g. [here](https://us-east-1.console.aws.amazon.com/bedrock/home?region=us-east-1#/modelaccess).   This workshop requires access to `Anthropic` for chat feature and `Titan` only for embeddings.   At the time of this writing `Anthropic` does require a use case justification.


For the workshop, the configuration in `/src/main/resources/application.properties` should contain the following and the rest in the file can be commented out.

```shell
spring.ai.bedrock.titan.embedding.enabled=true
spring.ai.bedrock.titan.embedding.model=amazon.titan-embed-text-v1
spring.ai.bedrock.titan.embedding.input-type=TEXT
spring.ai.bedrock.anthropic3.chat.enabled=true
```

## Workshop Overview

The workshop consists of six examples, each with a dedicated `README` file.  

All six workshop examples are organized into individual Java packages within this project. The code is essentially the same with every AI model. In each package, you'll find a Spring @RestController class that serves as the entry point for showcasing the discussed functionality.

To interact with the **@RestController**, you will be using the `http` utility as a user-friendly alternative to `curl`.

Detailed instructions and exercises for each example can be found in their respective README files:

* 1-README-tell-me-a-joke.md 
* 2-README-prompt-templating.md 
* 3-README-prompt-roles.md 
* 4-README-output-parser.md 
* 5-README-stuff-prompt.md 
* 6-README-retrieval-augmented-generation.md

Each README guide will walk you through the workshop exercises.

`NOTE:` before running the workshop, please visit [what is Spring](../main/0-README-what-is-spring.md)  to get some background on the Spring Framework.

