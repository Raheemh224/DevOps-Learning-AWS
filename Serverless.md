This is when developers don't manage servers anymore, just deploy code and functions. It is Function as a Service 

Serverless doesn’t mean there are no servers, it just means that you don’t manage/provision/see them. 

AWS lambda is what runs code for you 

DynamoDB is a fully managed serverless SQL database that scales automatically 

AWS Cognito – Helps manage user authentication 

AWS API Gateway – This acts as a bridge between the users and lambda functions  

Amazon S3 – Used to store files and content. Completely serverless and scales automatically based on the amount of data stored  

AWS SNS & SQS – These services help with communication between different parts of a system. SNS handles the notification and SQS for queuing messages  

AWS Kinesis Data Firehose  

Aurora Serverless – Database which auto scales based on demand 

Step Functions – Helps you manage workflows that have many Lambda functions 

Fargate  

Why Lambda? 

Virtual functions – no servers to manage 

Limited by time – short executions  

Run on-demand 

Scaling is automated  

Benefits? 

Easy pricing – pay per request and compute time 

Integrated with AWS services  

Integrated with many programming languages 

Easy monitoring with CloudWatch 

Increasing RAM with improve CPU and network  

Language support: 

Node.js 

Python  

Java  

C#/PowerShell 

Ruby 

Custom Runtime API 

 

Lambda Container Image  

The container image must implement the Lambda Runtime API 

ECS/Fargate is preferred for running arbitrary Docker Images  

 

 

 