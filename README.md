# Pep Talk Machine

> Feed it what you're up against. It prints back courage.

Pep Talk Machine is a small, single-purpose AI web app that generates short, personalized pep talks based on a situation you provide.

Instead of being a general-purpose chatbot, it focuses on one simple task: turning a few words of context into a concise and specific response.

## Features

- Generate personalized pep talks using AI
- Choose from preset situations
- Enter a custom situation
- Receive short, tailored responses
- Simple and responsive interface
- Serverless AWS architecture

## AWS Architecture

The application uses:

- **Amazon S3** — Hosts the static frontend
- **Amazon API Gateway** — Provides the HTTP API endpoint
- **AWS Lambda** — Handles the backend logic
- **Amazon Bedrock** — Generates responses using Nova Micro
- **IAM** — Provides the required Lambda permissions

For the complete architecture and request flow, see:

**[ARCHITECTURE.md](ARCHITECTURE.md)**

## How It Works

1. The user selects a preset situation or enters a custom one.
2. The frontend sends the situation to API Gateway.
3. API Gateway routes the request to Lambda.
4. Lambda builds the prompt and invokes Amazon Bedrock.
5. Nova Micro generates the pep talk.
6. Lambda returns the response to the frontend.
7. The generated pep talk is displayed to the user.

## Repository Contents

```text
pep-talk-machine/
│
├── ARCHITECTURE.md
├── README.md
├── cleanup-guide.md
├── deployment-guide.md
├── execution-workflow.md
├── frontend-explanation.md
└── index.html
````

### Documentation

| File                      | Description                                                |
| ------------------------- | ---------------------------------------------------------- |
| `ARCHITECTURE.md`         | Architecture diagram and explanation of the AWS components |
| `deployment-guide.md`     | Steps for deploying the application on AWS                 |
| `execution-workflow.md`   | Explains how a request moves through the application       |
| `frontend-explanation.md` | Explains the role of `index.html`                          |
| `cleanup-guide.md`        | Steps for cleaning up the AWS resources                    |

## Technologies

* HTML
* CSS
* JavaScript
* Python 3.13
* Amazon S3
* Amazon API Gateway
* AWS Lambda
* Amazon Bedrock
* IAM

## AWS Deploy Your First App Weekend Challenge

This project was built as part of the **AWS Deploy Your First App Weekend Challenge**.

The challenge focuses on building and deploying a real application on AWS, documenting the process, and sharing what was learned along the way.
