# Cleanup Guide

This guide explains how to clean up the AWS resources created for Pep Talk Machine after testing or completing the project.

## Resources to Remove

The application uses the following AWS resources:

- Amazon S3 bucket
- Amazon API Gateway HTTP API
- AWS Lambda function
- IAM execution role
- Amazon Bedrock model access

## 1. Remove the S3 Website

Open the Amazon S3 console and locate the bucket used for the Pep Talk Machine frontend.

Before deleting the bucket:

1. Remove the `index.html` file and any other objects.
2. Empty the bucket.
3. Delete the bucket.

Make sure you are deleting the correct bucket before confirming.

## 2. Delete the API Gateway API

Open **Amazon API Gateway**.

Locate the HTTP API used by Pep Talk Machine and delete it.

This removes the public API endpoint and its configured routes.

## 3. Delete the Lambda Function

Open **AWS Lambda**.

Locate the Pep Talk Machine Lambda function and delete it.

The function is no longer required once the API has been removed.

## 4. Remove the IAM Role

Open **IAM → Roles**.

Locate the execution role created for the Lambda function.

Delete the role only after confirming that it is not being used by another Lambda function or AWS resource.

## 5. Bedrock

The application does not create a separate Bedrock server or continuously running resource.

If model access was enabled specifically for this project, review your Amazon Bedrock model access settings as part of cleanup.

## Cleanup Order

A simple cleanup order is:

```text
API Gateway
     ↓
Lambda
     ↓
IAM Role
     ↓
S3 Bucket
     ↓
Review Bedrock Model Access
````

## Important

AWS resources can incur charges depending on usage and configuration.

Always verify the AWS console after cleanup to make sure the resources you created for the project have been removed.
