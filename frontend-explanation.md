# Frontend Explanation

## What is `index.html`?

`index.html` is the main frontend file of Pep Talk Machine.

It contains everything the browser needs to display and run the application's user interface, including the HTML structure, CSS styling, and JavaScript logic.

## What Does It Do?

When a user opens the application, the browser loads `index.html` from the Amazon S3 static website.

The file then:

1. Displays the Pep Talk Machine interface.
2. Provides preset situation buttons.
3. Allows the user to enter a custom situation.
4. Sends the user's situation to the API Gateway endpoint.
5. Shows a loading state while waiting for the response.
6. Receives the generated pep talk from the backend.
7. Displays the response to the user.

## How It Connects to AWS

The frontend does not directly communicate with Amazon Bedrock.

Instead, the request follows this path:

```text
index.html
    │
    │ HTTP POST
    ▼
API Gateway
    │
    ▼
Lambda
    │
    ▼
Amazon Bedrock
    │
    ▼
Generated Pep Talk
    │
    ▼
index.html
````

## HTML

The HTML defines the structure of the application.

It contains elements such as:

* The application title
* Situation input
* Preset situation buttons
* Generate button
* Loading indicator
* Response area
* Footer

## CSS

The CSS controls how the application looks.

It defines:

* Layout
* Typography
* Spacing
* Buttons
* Colors
* Animations
* Responsive behavior

This allows the application to have its own visual design without requiring a frontend framework.

## JavaScript

The JavaScript provides the application's functionality.

It handles:

* Preset selection
* User input
* Button interactions
* API requests
* Loading states
* Error handling
* Displaying the generated response

When the user clicks the generate button, JavaScript sends the situation to the API Gateway endpoint.

## API Request

The frontend sends the user's situation as an HTTP POST request.

For example:

```json
{
  "situation": "I have a job interview tomorrow."
}
```

The backend processes this request and returns the generated pep talk.

## Why Use a Single `index.html`?

The application is intentionally small, so a separate frontend framework or build system is not necessary.

Keeping the frontend in one file makes it:

* Easy to understand
* Easy to deploy
* Easy to host on Amazon S3
* Easy to modify
* Free from a frontend build step

The result is a lightweight frontend that can be served directly from S3.

