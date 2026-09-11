# Deployment Guide

This guide explains how to deploy Pep Talk Machine using the AWS Management Console.

The application uses a serverless architecture:

```text
Amazon S3
    ↓
API Gateway
    ↓
AWS Lambda
    ↓
Amazon Bedrock
````

## Prerequisites

Before starting, make sure you have:

* An AWS account
* Access to the AWS Management Console
* The Pep Talk Machine `index.html` file
* Access to the Amazon Bedrock Nova Micro model

## 1. Create the S3 Bucket

Open the **Amazon S3** console.

Create a bucket for the application.

Upload:

```text
index.html
```

Enable static website hosting for the bucket.

Configure the website to use:

```text
index.html
```

as the index document.

The S3 website endpoint will be used to access the frontend.

## 2. Create the Lambda Function

Open **AWS Lambda**.

Create a new Lambda function.

Use:

```text
Runtime: Python 3.13
```

Create a new execution role with the required Lambda permissions.

Add the backend code for Pep Talk Machine.

The Lambda function is responsible for:

1. Receiving the user's situation.
2. Building the prompt.
3. Calling Amazon Bedrock.
4. Returning the generated pep talk.

## 3. Configure Bedrock Permissions

The Lambda execution role needs permission to invoke the Bedrock model.

Attach the required Bedrock permissions to the Lambda execution role.

The permission flow is:

```text
IAM Role
    ↓
AWS Lambda
    ↓
Amazon Bedrock
```

## 4. Request Bedrock Model Access

Open **Amazon Bedrock** and verify that the required model access is available.

The application uses:

```text
Amazon Bedrock
Nova Micro
```

Model access should be confirmed before testing the complete application.

## 5. Test Lambda

Before connecting API Gateway, test the Lambda function independently.

This helps verify that:

* Lambda receives the expected input.
* The prompt is created correctly.
* Bedrock can be invoked.
* Nova Micro returns a response.
* Lambda formats the response correctly.

Testing this layer independently makes later troubleshooting easier.

## 6. Create the API Gateway HTTP API

Open **Amazon API Gateway**.

Create an **HTTP API**.

Configure a route for the Lambda function.

The frontend sends an HTTP POST request to this route.

The flow becomes:

```text
Browser
   ↓
API Gateway
   ↓
Lambda
   ↓
Bedrock
```

## 7. Configure CORS

Configure CORS for the HTTP API so that the browser can make requests to API Gateway.

For a simple public frontend, configure the appropriate origin and HTTP methods required by the application.

After changing CORS or other API settings, make sure the changes are deployed to the active stage.

If auto-deploy is disabled, saving the configuration does not automatically update the live API.

## 8. Update the Frontend API URL

The frontend needs the API Gateway endpoint.

The URL should include the API's route path.

For example:

```text
https://<api-id>.execute-api.<region>.amazonaws.com/<route>
```

Do not use the API Gateway ARN as the browser endpoint.

## 9. Deploy and Test

Open the S3 website URL in a browser.

Test the application by:

1. Selecting a preset situation.
2. Clicking the generate button.
3. Entering a custom situation.
4. Confirming that a response is returned.

If the request fails, check the following layers separately:

```text
Browser
   ↓
API Gateway
   ↓
Lambda
   ↓
IAM
   ↓
Bedrock
```
