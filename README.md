# FileEncrypter

A serverless PDF encryption platform built on AWS that enables authenticated users to securely upload PDF documents, automatically encrypt them using AWS Lambda, and download the encrypted versions through a modern Angular web application.

## Overview

FileEncrypter is an event-driven cloud application that demonstrates the use of AWS serverless services to build a scalable and secure document processing workflow.

Users can register and authenticate using Amazon Cognito, upload PDF files directly to Amazon S3 using pre-signed URLs, and automatically encrypt documents through AWS Lambda. The application tracks processing status in DynamoDB and provides secure download access to encrypted files.

---

## Features

### Authentication & Authorization

* User registration with email verification
* Secure login using Amazon Cognito User Pool
* Route protection using Angular Guards
* User-specific file ownership and isolation

### File Upload

* Direct browser-to-S3 uploads using pre-signed URLs
* No file transfer through backend servers
* Reduced latency and infrastructure costs

### Automated PDF Encryption

* Event-driven processing using S3 Event Notifications
* Automatic PDF encryption using Python and PyPDF
* Secure storage of encrypted documents

### File Management

* Track file processing status
* View uploaded files
* Download encrypted PDFs
* User-specific file listings

### Serverless Architecture

* AWS Lambda functions for backend processing
* Amazon API Gateway for REST APIs
* Amazon DynamoDB for metadata storage
* Amazon S3 for document storage
* Infrastructure as Code using AWS SAM

---

## Architecture

### High-Level Workflow

```text
User
 │
 ▼
Angular Frontend (AWS Amplify)
 │
 ▼
Amazon Cognito
 │
 ▼
API Gateway
 │
 ├──────────────► GenerateUploadUrl Lambda
 │                       │
 │                       ▼
 │                DynamoDB Record
 │                       │
 │                       ▼
 │                Pre-Signed URL
 │
 ▼
Upload PDF
 │
 ▼
Amazon S3 Source Bucket
 │
 ▼
S3 Event Trigger
 │
 ▼
EncryptPDF Lambda
 │
 ├────────► Encrypt PDF
 │
 ├────────► Upload Encrypted PDF
 │
 └────────► Update DynamoDB Status
                     │
                     ▼
           Amazon S3 Destination Bucket

User
 │
 ▼
Files Page
 │
 ▼
file_list Lambda
 │
 ▼
DynamoDB

User
 │
 ▼
Download File
 │
 ▼
GenerateDownloadUrl Lambda
 │
 ▼
Pre-Signed Download URL
 │
 ▼
Encrypted PDF
```

---

## Technology Stack

### Frontend

* Angular 21
* TypeScript
* HTML5
* CSS3
* AWS Amplify Hosting

### Backend

* AWS Lambda
* Python 3.12
* AWS API Gateway
* AWS SAM (Serverless Application Model)

### Storage & Database

* Amazon S3
* Amazon DynamoDB

### Authentication

* Amazon Cognito User Pools

### PDF Processing

* PyPDF

---

## AWS Services Used

| Service         | Purpose                |
| --------------- | ---------------------- |
| Amazon Cognito  | User Authentication    |
| API Gateway     | REST API Management    |
| AWS Lambda      | Serverless Compute     |
| Amazon DynamoDB | Job Metadata Storage   |
| Amazon S3       | File Storage           |
| AWS Amplify     | Frontend Hosting       |
| AWS SAM         | Infrastructure as Code |
| CloudWatch      | Logging & Monitoring   |

---

## Lambda Functions

### GenerateUploadUrl

Generates a pre-signed S3 upload URL and creates a processing record in DynamoDB.

**Responsibilities**

* Generate Job ID
* Store metadata in DynamoDB
* Generate secure upload URL

---

### EncryptPDF

Triggered automatically when a file is uploaded to the source bucket.

**Responsibilities**

* Download uploaded PDF
* Encrypt PDF using PyPDF
* Upload encrypted file
* Update DynamoDB status

---

### file_list

Retrieves file records for a specific user.

**Responsibilities**

* Query DynamoDB
* Return file list and processing status

---

### GenerateDownloadUrl

Generates a secure pre-signed URL for downloading encrypted PDFs.

**Responsibilities**

* Retrieve encrypted file location
* Generate temporary download URL

---

## DynamoDB Data Model

### EncryptionJobs Table

```json
{
  "jobId": "049319ef-c635-4376-8f12-09e276a37a71",
  "userId": "b103cd8a-2041-70a3-1bbe-7238eb4c227c",
  "fileName": "Lecture-1.pdf",
  "fileKey": "uploads/userId/jobId/Lecture-1.pdf",
  "encryptedKey": "uploads/userId/jobId/Lecture-1_encrypted.pdf",
  "status": "COMPLETED"
}
```

---

## Project Highlights

* Designed and implemented a complete serverless architecture on AWS.
* Implemented direct S3 uploads using pre-signed URLs to eliminate backend file transfer overhead.
* Built an event-driven document processing pipeline using S3 triggers and AWS Lambda.
* Implemented user-specific file isolation using Cognito identities and DynamoDB indexing.
* Automated PDF encryption using Python and PyPDF.
* Hosted the Angular frontend using AWS Amplify with continuous deployment from GitHub.
* Managed cloud infrastructure using AWS SAM and CloudFormation.

---

## Learning Outcomes

This project provided hands-on experience with:

* Serverless Application Development
* Event-Driven Architecture
* Cloud Security and Authentication
* Infrastructure as Code (IaC)
* AWS Lambda Development
* API Design and Integration
* Angular Frontend Development
* Cloud Storage and Database Design
* CI/CD Deployment with AWS Amplify

---

## Future Improvements

* Cognito Authorizer integration for API Gateway
* User-defined encryption passwords
* Email notifications after processing
* File deletion and management
* Processing progress indicators
* Custom domain configuration
* Multi-file upload support
* Audit logging and activity tracking

---

## Author

**Isuranga Dasun**

BSc (Hons) in Software Engineering
Department of Software Engineering
Sabaragamuwa University of Sri Lanka

GitHub: https://github.com/isuranga-2002

---

**Project Type:** Serverless Cloud Application
**Deployment:** AWS Amplify + AWS Serverless Stack

## Project Links

### Live Application

https://master.d2ij40r38juaaj.amplifyapp.com

### Frontend Repository

https://github.com/isuranga-2002/file_encrypter_frontend

### Backend Repository

https://github.com/isuranga-2002/file_encrypter

### Architecture Documentation

https://github.com/isuranga-2002/file_encrypter_project
