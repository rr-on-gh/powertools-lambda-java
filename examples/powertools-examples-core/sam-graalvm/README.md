#  Powertools for AWS Lambda (Java) - Core Utilities Example with SAM on GraalVM

This project demonstrates the Lambda for Powertools Java module deployed using [Serverless Application Model](https://aws.amazon.com/serverless/sam/).

For general information on the deployed example itself, you can refer to the parent [README](../README.md)

## Configuration
SAM uses [template.yaml](template.yaml) to define the application's AWS resources.
This file defines the Lambda function to be deployed as well as API Gateway for it.

## Deploy the sample application
- Build the Docker image which will be used as the environment where the SAM build would run:
```shell
       docker build --platform linux/amd64 . -t powertools-examples-core-sam-graalvm
```

- Build the SAM project using the docker image
```shell
    sam build --use-container --build-image sam/custom-graal-image 

```

- Build the binary without SAM build. This is needed during development when SNAPSHOT JARs are not available for download from mvn central from the docker image
```shell
       docker run --platform linux/amd64  -it -v `pwd`:`pwd` -w `pwd` -v ~/.m2:/root/.m2 powertools-examples-core-sam-graalvm mvn clean -Pnative-image package -DskipTests
       mkdir -p .aws-sam/build/HelloWorldFunction/
       cp target/hello-world .aws-sam/build/HelloWorldFunction/
       cp src/main/config/bootstrap .aws-sam/build/HelloWorldFunction/
       chmod +x .aws-sam/build/HelloWorldFunction/bootstrap
       chmod +x .aws-sam/build/HelloWorldFunction/hello-world
```

- SAM deploy

       sam deploy

To deploy the example, check out the instructions for getting
started with SAM in [the examples directory](../../README.md)

## Additional notes

You can watch the trace information or log information using the SAM CLI:
```bash
# Tail the logs
sam logs --tail $MY_STACK

# Tail the traces
sam traces --tail
```