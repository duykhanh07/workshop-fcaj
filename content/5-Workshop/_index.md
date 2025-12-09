---
title: "Workshop"
#date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building **AI Career Coach** - a modern Fullstack Serverless application designed to help users enhance their career opportunities through the power of Artificial Intelligence

---

## 🎥 Product Demo

[Watch video](https://www.youtube.com/watch?v=I2NEzLo2n7s)

---

## 💡 Problem & Solution

### The Problem
The job market is becoming increasingly competitive. Candidates often face difficulties with:
* Writing a standard, **ATS-optimized** CV (Resume).
* Drafting personalized **Cover Letters** for specific companies.
* Lacking an environment to **practice interviews** and receive objective feedback.

### The Solution: AI Career Coach
This project addresses these issues by utilizing **Amazon Bedrock (Claude 3)** to act as a virtual career coach:
1.  **AI Resume Builder:** Assists in writing, editing, and optimizing professional CV content.
2.  **Cover Letter Generator:** Automatically generates cover letters based on Job Descriptions (JD) and user profiles.
3.  **Mock Interview:** Conducts mock interviews with AI tailored to specific industries and provides scoring/feedback.

---

## 🛠️ Tech Stack

The project adopts a **Serverless Microservices** architecture, optimizing costs and scalability.

### 1. Frontend
* **React (Vite):** Fast development speed and high performance.
* **Tailwind CSS & Shadcn UI:** For building beautiful, modern, and accessible user interfaces.
* **AWS Amplify:** Manages Authentication (Cognito) and API connections.

### 2. Backend (Java Serverless)
* **Java 17 & Spring Boot:** A robust backend platform.
* **Spring Cloud Function:** Converts Java logic into Serverless functions.
* **AWS Lambda:** Runs code without server management. Includes 5 distinct functions:
    * `UserProfileFunction`
    * `ResumeFunction`
    * `CoverLetterFunction`
    * `InterviewFunction`
    * `IndustryInsightFunction`

### 3. Database & AI
* **Amazon DynamoDB:** NoSQL database using high-performance **Single Table Design**.
* **Amazon Bedrock:** The gateway to Foundation Models. We utilize the **Claude 3 Haiku** model from Anthropic.

### 4. Infrastructure & DevOps
* **AWS SAM (Serverless Application Model):** Defines the entire infrastructure as code (IaC).
* **Amazon S3 & CloudFront:** Global storage and content delivery for the Frontend with OAC security.

---

## 🎯 Workshop Goals

By the end of this workshop, you will master:
1.  How to build a complete **Fullstack** application.
2.  **Serverless** system design thinking on AWS.
3.  **Prompt Engineering** techniques to integrate AI into Java applications.
4.  Securing applications with **JWT** and **Cognito**.
5.  Basic **CI/CD** processes: Build, Deploy, and Hosting.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequiste](5.2-Prerequiste/)
3. [Infrastructure as Code](5.3-Infrastructure-as-code/)
4. [Backend Development](5.4-Backend-development/)
5. [Frontend Development](5.5-Frontend-development/)
6. [Deploy](5.6-Deploy/)
7. [Clean up](5.6-Cleanup/)