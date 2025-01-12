# AWS Lambda to AWS Lambda - Create a Lambda function that consumes events

This pattern is a Lambda function asynchronously triggered by a Lambda Function. 

The SAM template deploys three Lambda functions. A Producer function invokes a failure handler Lambda function on failure, or on Success handler Lambda function on success using event destinations.

Learn more about this pattern at Serverless Land Patterns: https://serverlessland.com/patterns/lambda-lambda

Important: this application uses various AWS services and there are costs associated with these services after the Free Tier usage - please see the [AWS Pricing page](https://aws.amazon.com/pricing/) for details. You are responsible for any AWS costs incurred. No warranty is implied in this example.

## Requirements

* [Create an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) installed and configured
* [Git Installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) (AWS SAM) installed

## Deployment Instructions

1. Create a new directory, navigate to that directory in a terminal and clone the GitHub repository:
    ``` 
    git clone https://github.com/aws-samples/serverless-patterns
    ```
1. Change directory to the pattern directory:
    ```
    cd lambda-lambda
    ```
1. From the command line, use AWS SAM to deploy the AWS resources for the pattern as specified in the template.yml file:
    ```
    sam deploy --guided
    ```
1. During the prompts:
    * Enter a stack name
    * Enter the desired AWS Region
    * Allow SAM CLI to create IAM roles with the required permissions.

    Once you have run `sam deploy -guided` mode once and saved arguments to a configuration file (samconfig.toml), you can use `sam deploy` in future to use these defaults.

1. Note the outputs from the SAM deployment process. These contain the resource names and/or ARNs which are used for testing.

## How it works

* Use the AWS CLI to asynchronously invoke the producer function.
* A successfull execution will asynchronously invoke the `successHandlerFunction`
* An unsuccessfull, or failed execution will asynchronously invoke the `failHandlerFunction`
* which you can see in CloudWatch Logs.

==============================================

## Testing

### Success Testing

```bash
aws lambda invoke --function-name producerFunction --invocation-type Event --payload  '{"Success":true}' response.json --cli-binary-format raw-in-base64-out
```

### Failure testing
```bash
aws lambda invoke --function-name producerFunction --invocation-type Event --payload  '{"Success":false}' response.json --cli-binary-format raw-in-base64-out
```

## Cleanup
 
1. Delete the stack
    ```bash
    aws cloudformation delete-stack --stack-name STACK_NAME
    ```
1. Confirm the stack has been deleted
    ```bash
    aws cloudformation list-stacks --query "StackSummaries[?contains(StackName,'STACK_NAME')].StackStatus"
    ```
----
Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.

SPDX-License-Identifier: MIT-0
