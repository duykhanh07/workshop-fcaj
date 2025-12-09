---
title: "Proposal"
#date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# AI Career Coach   

### 1. Executive Summary  
The AI Career Coach platform is designed for job seekers and fresh graduates to optimize the application process and enhance job opportunities. The system operates as an intelligent virtual assistant, supporting from building profiles (CV), writing cover letters to interview practice. The platform leverages the power of AWS Serverless to ensure automatic scalability and Amazon Bedrock (Generative AI) for deep content personalization. The system ensures user personal information security through Amazon Cognito. 

### 2. Problem Statement  
*Current Problem*  
Job seekers currently spend too much time manually editing CVs for each application position. Writing cover letters is often stereotypical, lacking highlights. Additionally, providing candidates with an environment to practice professional knowledge and analyze industry insights regarding their own career as quickly as possible.  

*Solution*  
The platform uses a comprehensive AWS Serverless architecture to solve the above problems:
* Amazon S3 & CloudFront: Store and distribute the web interface (SPA) at high speed.
* Amazon Cognito: Manage identity and authenticate users securely.
* Amazon API Gateway & AWS Lambda (Java Spring Cloud Function): Handle business logic according to Microservices architecture (User, Resume, Cover Letter, Interview).
* Amazon DynamoDB: Store profile data, assessment tests, and industry information with low latency.
* Amazon Bedrock: The heart of the system, providing the ability to upgrade CVs, draft Cover Letters, and generate interview questions according to real-world contexts.

The platform focuses deeply on the "AI Personalization" feature; users not only store CVs but are also supported by AI. Key features include: CV upgrading, AI-based cover letter writing based on JD, taking knowledge review quizzes, industry trend reports.  

*Benefits and Return on Investment (ROI)*  
Reduce a lot of time writing cover letters and editing CVs. Leverage AWS Free Tier for Lambda, DynamoDB, and Cognito; costs mainly come from Bedrock API calls. Estimated operating cost is about 3-5 USD/month for personal use or small scale (under 1000 AI requests/month). The biggest value lies not in direct cash but in shortening cover letter writing and CV editing. No initial hardware costs. 

### 3. Solution Architecture  
The platform applies a fully AWS Serverless architecture to optimize scalability and maintenance costs. The user interface is distributed globally through Amazon CloudFront and S3. The backend system uses Microservices architecture with AWS Lambda (Java Spring Cloud Function) to handle business logic and Amazon Bedrock to integrate artificial intelligence. Profile and assessment data is centrally managed by Amazon DynamoDB, ensuring high performance and security.  

![AI Career Coach Architecture](/images/2-Proposal/AI_Career_Coach_Architecture.png)

*AWS Services Used*
- *Amazon CloudFront & S3*: Store and distribute Web interface with low latency.
- *Amazon Cognito*: Manage identity, sign-up/sign-in, and secure user authentication.
- *Amazon API Gateway*: REST API gateway managing traffic and routing requests to the correct Lambda Service.
- *AWS Lambda*: Handle business logic (5 services: User, Resume, Cover Letter, Interview, Industry) on Java Spring environment.
- *Amazon DynamoDB*: NoSQL database storing user information, resumes, and assessment history (1 tables).
- *Amazon Bedrock*: Provide foundation models (Claude 3 Haiku/Sonnet) to analyze and generate content. 

*Component Design*
- *User Interface (Frontend)*: Next.js Single Page Application (SPA) interacting with backend through RESTful APIs.
- *Access Management*: Amazon Cognito (User Pool) authenticates users and issues JWT Tokens for API requests.
- *Central Processing*: AWS Lambda executes Spring Cloud Function functions, connecting with Bedrock to handle intelligent tasks (creating quizzes, fixing CVs).
- *Data Layer*: DynamoDB is organized using a 'single-table' design to optimize data organization and enable efficient querying.
- *Artificial Intelligence*: Amazon Bedrock receives context from Lambda, performs inference, and returns consultation results or text content. 

### 4. Technical Implementation
*Implementation Phases*
The project is divided into 2 major phases, focusing on designing optimal Serverless architecture for Java and integrating Generative AI:
1. *Research, Design, and Feasibility Assessment*: Design AWS Serverless data flow diagrams, define Microservices separation strategy with Spring Cloud Function. Select suitable AI model (Claude 3 Haiku) based on accuracy and speed benchmarks. Use AWS Pricing Calculator to estimate costs for Lambda (Java runtime), DynamoDB (Read/Write capacity), and most importantly, Amazon Bedrock Token costs. Set up expected budget (AWS Budget) to ensure the project stays within Free Tier limits or lowest cost. (Month 2)
2. *Development, Optimization, and Operation*: Build Lambda functions using Java (Spring Boot 3), configure DynamoDB Multi-table, and write Prompt Engineering for Bedrock to optimize output results for CV/Interview. Integrate Frontend (Next.js) with API Gateway and Cognito, perform Integration Test for the whole system before Go-live. (Month 3)

*Technical Requirements*
- *Backend Services (Java Serverless):* Proficient in Java 17/21 and Spring Cloud Function to write code according to the functional model. Deep understanding of AWS Lambda SnapStart mechanism to optimize Java application startup time (reducing latency from seconds to milliseconds).
- *Generative AI & Data:* Advanced Prompt Engineering skills to control Claude 3 model on Amazon Bedrock to return accurate JSON format. Effective DynamoDB schema design (Partition Key/Sort Key) for assessment history and user profile queries.
- *Infrastructure & Security:* Use Amazon Cognito to manage User Pool and JWT authentication. Configure Amazon API Gateway to map requests and handle CORS for Frontend. Deploy Frontend (Next.js) to Amazon S3 and distribute via CloudFront.

### 5. Roadmap & Milestones   
- *Internship (Month 1–3)*:  
    - Month 1: Learn AWS and upgrade hardware.  
    - Month 2: Design and adjust architecture.  
    - Month 3: Deploy, test, put into use.  
- *Post-deployment*: Research further to expand more functions.  

### 6. Budget Estimation  
Costs can be viewed on [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=16a1acd5f6d2fe1cf2414547f30fdfb0504f8c0d)  

*Infrastructure Costs (Region: Asia Pacific - Singapore)*
- Amazon Bedrock: 2.70 USD/month (AI model, processing ~1,000 token input/output per request).
- Amazon CloudFront: 0.85 USD/month (Data transfer to internet 10 GB).
- Amazon DynamoDB: 0.28 USD/month (Storage 1 GB, Standard mode).
- AWS Lambda: 0.13 USD/month (4,000 requests, 512 MB temporary memory).
- Amazon S3: 0.05 USD/month (Storage 1 GB, 4,000 PUT/GET requests).
*Total*: 4.01 USD/month, equivalent to 48.12 USD/12 months.

### 7. Risk Assessment
*Risk Matrix*
- AI Hallucination: High Impact, Medium Probability. (AI gives misleading advice or fabricates information in CV).
- Cost Overrun: Medium Impact, Low Probability. (Due to Bedrock token fees if request volume increases suddenly).
- System Latency (Cold Start): Medium Impact, Low Probability. (Due to the nature of Java Lambda functions when starting up).

*Mitigation Strategies*
- Accurate AI: Optimize Prompt Engineering with specific context, set low `Temperature` parameters (0.1 - 0.2) to reduce randomness. Add disclaimer warnings for users.
- Cost Control: Set up AWS Budgets to alert when costs reach 80% of the allowed threshold.
- Performance: Enable AWS Lambda SnapStart feature to reduce Java startup time from seconds to milliseconds.

*Contingency Plan*
- AI Service Incident: Design the system to "fail gracefully" (friendly error notification) or return available templates if Amazon Bedrock is interrupted.
- Data Recovery: Enable DynamoDB Point-in-Time Recovery (PITR) feature to restore user data in case of accidental deletion or application errors.

### 8. Expected Outcomes
*Technical Improvements:* Automate 90% of the application profile preparation process (from editing CVs to writing cover letters), completely replacing time-consuming manual drafting. The system achieves low latency (milliseconds) thanks to Java SnapStart optimization, capable of handling high loads without infrastructure intervention.
*Long-term Value:* Successfully build a sample architecture framework for Java Serverless applications combined with GenAI on AWS, reusable for enterprise projects. Create a real-world environment to test and refine complex Prompt Engineering techniques, serving the specialized development direction of AI Engineer.