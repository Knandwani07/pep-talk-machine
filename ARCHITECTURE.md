# Architecture Overview

Pep Talk Machine uses a simple serverless architecture where each AWS service handles a specific part of the application.

## Architecture

<img width="2048" height="1028" alt="export" src="https://github.com/user-attachments/assets/afd8ae2e-b553-49cd-87fd-1ff4c75bb4cb" />


## Request Flow

The application follows this flow:

```text
User / Browser
      │
      ├── Page Load
      ▼
Amazon S3
Static Website Hosting
      │
      │ HTTP POST Request
      ▼
Amazon API Gateway
HTTP API
      │
      ▼
AWS Lambda
Python 3.13
      │
      │ Invoke Model
      ▼
Amazon Bedrock
Nova Micro
      │
      ▼
Generated Pep Talk
      │
      ▼
AWS Lambda
      │
      ▼
Amazon API Gateway
      │
      ▼
User / Browser
```

## Amazon S3

Amazon S3 hosts the static frontend of Pep Talk Machine.

The frontend is a single HTML file containing the application's HTML, CSS, and JavaScript. When a user opens the application, the browser loads this file from the S3 static website endpoint.

S3 is responsible for serving the frontend and is not involved in generating the pep talk.

## Amazon API Gateway

Amazon API Gateway provides the HTTP API that connects the frontend to the backend.

When the user submits a situation, the browser sends an HTTP POST request to the API Gateway endpoint.

API Gateway receives the request and routes it to the Lambda function.

## AWS Lambda

AWS Lambda contains the backend logic and runs using Python 3.13.

The Lambda function:

1. Receives the request from API Gateway.
2. Parses the user's situation.
3. Builds the prompt.
4. Invokes the Amazon Bedrock model.
5. Processes the generated response.
6. Returns the response to API Gateway.

Lambda is invoked only when a request is received, so there is no continuously running backend server.

## Amazon Bedrock

Amazon Bedrock provides the AI model used by Pep Talk Machine.

The application uses **Nova Micro** to generate the pep talk.

Lambda sends the user's situation and prompt instructions to Bedrock using the model invocation API. Bedrock then generates a short response tailored to the user's situation.

## IAM

IAM controls the permissions required by the Lambda function.

The Lambda execution role has permission to invoke the Bedrock model.

```text
IAM Role
    │
    │ Invoke Permission
    ▼
AWS Lambda ──────────► Amazon Bedrock
```

The frontend does not need AWS credentials to access Bedrock. The Lambda execution role handles the required AWS permissions.

## Why Serverless?

The application has a simple workload:

* Serve a static frontend.
* Receive an HTTP request.
* Generate text using an AI model.
* Return the response.

Because of this, there is no need for a continuously running application server.

Each AWS service has a focused responsibility:

```text
S3          → Hosts the frontend
API Gateway → Receives HTTP requests
Lambda      → Handles backend logic
Bedrock     → Generates the pep talk
IAM         → Controls permissions
```

This keeps the architecture small, focused, and easy to test layer by layer.
