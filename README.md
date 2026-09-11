# Pep Talk Machine

> Feed it what you're up against. It prints back courage.

Pep Talk Machine is a small, single-purpose AI web app that generates short, personalized pep talks based on a situation you provide.

Instead of being a general-purpose chatbot, it focuses on one thing: turning a few words of context into a concise and specific response.

## Live Demo

[Launch Pep Talk Machine](http://pep-talk-ai-aws-builder-center-weekend-challenge.s3-website-us-east-1.amazonaws.com/)

## Features

- Generate personalized pep talks using AI
- Choose from preset situations
- Enter a custom situation
- Receive short, tailored responses
- Simple and responsive interface
- Fully serverless AWS architecture

## AWS Architecture

The application uses Amazon S3, Amazon API Gateway, AWS Lambda, Amazon Bedrock, and IAM.

For the complete architecture diagram and explanation, see:

[Architecture Overview](ARCHITECTURE.md)

## AWS Services Used

| Service | Purpose |
|---|---|
| Amazon S3 | Hosts the static frontend |
| Amazon API Gateway | Provides the HTTP API endpoint |
| AWS Lambda | Processes requests and invokes Bedrock |
| Amazon Bedrock | Generates pep talks using Nova Micro |
| IAM | Provides Lambda with permission to invoke Bedrock |

## How It Works

1. The user selects a preset situation or enters a custom situation.
2. The frontend sends an HTTP POST request to API Gateway.
3. API Gateway routes the request to AWS Lambda.
4. Lambda builds the prompt and invokes Amazon Bedrock.
5. Nova Micro generates the pep talk.
6. Lambda returns the generated response.
7. The frontend displays the pep talk to the user.

## Project Structure

```text
pep-talk-machine/
├── index.html
├── README.md
├── ARCHITECTURE.md
├── LICENSE
├── .gitignore
└── docs/
    └── architecture.png
