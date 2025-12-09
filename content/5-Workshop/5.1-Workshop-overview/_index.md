---
title : "Introduction"
#date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

## 1. What is the "AI Career Coach" Application?

**AI Career Coach** is a platform designed to support job seekers, leveraging the power of **Generative AI** to streamline the most time-consuming tasks in the application process.

Below are the 4 main feature modules that we will build:

### A. Dashboard
Where users get an overview of their industry/profession.

![dashboard](/images/5-Workshop/5.1-Workshop-overview/dashboard.png)

### B. AI Resume Builder
An AI-integrated Markdown editor. Users input raw information, and the AI helps rephrase work experience descriptions to be professional and **ATS (Applicant Tracking System)** compliant.

![resume builder form](/images/5-Workshop/5.1-Workshop-overview/resume-builder-form.jpeg)
![resume builder markdown](/images/5-Workshop/5.1-Workshop-overview/resume-builder-markdown.jpeg)

### C. Cover Letter Generator
Users only need to provide: "Job Position", "Company Name", and "Job Description (JD)". The system will call Claude 3 to write a persuasive cover letter, personalized to that specific JD.

![cover letter page](/images/5-Workshop/5.1-Workshop-overview/cover-letter-page.jpeg)
![cover letter form](/images/5-Workshop/5.1-Workshop-overview/cover-letter-form.jpeg)

### D. Mock Interview
The most interesting feature. The system generates multiple-choice questions based on the user's industry. After the user answers, the system scores the performance and provides improvement advice.

![interview](/images/5-Workshop/5.1-Workshop-overview/interview-page.jpeg)

---

## 2. System Architecture

This is the "backbone" of the workshop. We will apply a fully **Serverless** architecture on AWS.

Please look at the data flow diagram below:

![AI Career Coach Architecture](/images/2-Proposal/AI_Career_Coach_Architecture.png)

### Data Flow Analysis:

1.  **Frontend Hosting:** React source code (built) is stored on **Amazon S3** and distributed globally via **Amazon CloudFront** to ensure the fastest page load speeds.
2.  **Authentication:** Users log in via **Amazon Cognito**. The Frontend receives a JWT Token to authenticate requests sent to the Server.
3.  **API Routing:** **Amazon API Gateway** acts as the entry point, receiving HTTP Requests from the Frontend, checking the Token (Authorizer), and routing to the correct handling Lambda Function.
4.  **Backend Logic (Compute):** We use 5 **AWS Lambda** functions written in **Java 17 (Spring Cloud Function)**. Each function handles a distinct business logic (Microservices pattern).
5.  **Database:** Data is stored in **Amazon DynamoDB** using **Single Table Design** (sharing 1 table for User, Resume, Interview, etc.), optimized for extremely fast read/write performance.
6.  **AI Integration:** Lambda Functions requiring intelligence will call **Amazon Bedrock** to interact with the **Claude 3 Haiku** model.

---

## 3. Why Choose This Tech Stack?

Why combine Java Spring Boot with Serverless?

* **Java & Spring Cloud Function:** Java is a popular enterprise language. Spring Cloud Function helps Java developers easily transition familiar Spring Boot code to a Serverless environment without relearning from scratch.
* **AWS Lambda & SnapStart:** Previously, Java was criticized for slow startup (Cold Start). With **Lambda SnapStart**, our Java code will start extremely fast, almost instantly.
* **DynamoDB Single Table:** This is an advanced DB design technique that helps reduce costs and increase query speed by minimizing data joins (which do not exist in NoSQL).
* **Bedrock:** Instead of managing expensive GPU servers to run AI, Bedrock provides an API to call top-tier models (like Claude, Llama) on a Serverless basis (pay-as-you-go).

---

## 4. Source Code Structure

This project is organized as a **Monorepo** (containing both Frontend and Backend in the same repository) for easy management during the workshop.

![source code](/images/5-Workshop/5.1-Workshop-overview/source-code.png)

* `backend/`: Contains Java Spring Boot source code.
* `frontend/`: Contains React Vite source code.
* `template.yaml`: The Infrastructure as Code definition file for AWS SAM.