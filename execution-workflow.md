# Execution Workflow

This document explains how a request moves through Pep Talk Machine from the moment the user submits a situation until the generated pep talk appears in the browser.

## Complete Workflow

```text
User
 │
 │ Enters situation
 ▼
Frontend
 │
 │ HTTP POST
 ▼
API Gateway
 │
 │ Routes request
 ▼
Lambda
 │
 │ Builds prompt
 ▼
Amazon Bedrock
 │
 │ Nova Micro generates response
 ▼
Lambda
 │
 │ Formats response
 ▼
API Gateway
 │
 │ HTTP response
 ▼
Frontend
 │
 ▼
User sees pep talk
````

## Step 1: User Input

The user opens Pep Talk Machine in a browser.

They can either select a preset situation or enter their own.

Example:

```text
I have a job interview tomorrow.
```

The frontend prepares this information for the API request.

## Step 2: HTTP POST Request

The frontend sends an HTTP POST request to the API Gateway endpoint.

The request contains the user's situation.

Example:

```json
{
  "situation": "I have a job interview tomorrow."
}
```

## Step 3: API Gateway

Amazon API Gateway receives the HTTP request.

The configured route sends the request to the Lambda function.

API Gateway acts as the entry point between the public frontend and the serverless backend.

## Step 4: Lambda Processing

AWS Lambda receives the request.

The function:

1. Parses the incoming request.
2. Extracts the situation.
3. Builds the application prompt.
4. Sends the prompt to Amazon Bedrock.

Lambda is responsible for the application's backend logic.

## Step 5: Bedrock Model Invocation

Lambda invokes Amazon Bedrock using the configured model.

The application uses:

```text
Amazon Bedrock
Nova Micro
```

The model receives the relevant user context and prompt instructions.

The prompt is designed to produce a short, specific pep talk rather than a generic motivational response.

## Step 6: Generated Response

Nova Micro generates the response.

For example:

```text
You already did the work that got you into the room. Now focus on showing them how you think and let the preparation speak for itself.
```

The generated text is returned to Lambda.

## Step 7: Lambda Response

Lambda formats the generated result into the response expected by the frontend.

Example:

```json
{
  "pep_talk": "You already did the work that got you into the room. Now focus on showing them how you think and let the preparation speak for itself."
}
```

## Step 8: API Gateway Response

API Gateway returns the Lambda response to the browser.

The frontend receives the generated pep talk.

## Step 9: Frontend Display

The JavaScript code processes the API response and displays the generated pep talk to the user.

The complete interaction happens without the browser directly accessing Amazon Bedrock.

## Security and Permissions

The browser does not need AWS credentials to invoke Bedrock.

The permission flow is handled through the Lambda execution role:

```text
Browser
   │
   ▼
API Gateway
   │
   ▼
Lambda
   │
   │ IAM permission
   ▼
Amazon Bedrock
```

IAM controls what the Lambda function is allowed to access.

## Why Test Each Layer Separately?

Testing the components independently makes it easier to identify where a failure occurs.

For example, if Lambda can successfully invoke Bedrock during a standalone test, then a later browser failure is more likely to be related to API Gateway, CORS, routing, or the frontend rather than the Bedrock integration.

The same approach can be applied throughout the architecture:

```text
S3
 ↓
Frontend works?

API Gateway
 ↓
Request reaches route?

Lambda
 ↓
Function executes?

IAM
 ↓
Permission exists?

Bedrock
 ↓
Model responds?
```

This layer-by-layer approach was particularly useful while deploying Pep Talk Machine because several of the issues appeared to be application problems but were actually caused by configuration, routing, or deployment details.


