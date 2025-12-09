---
title: "Worklog Week 11"
#date: "`r Sys.Date()`"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Week 11 Objectives:
* Finalize Serverless system architecture design on AWS (S3, CloudFront, API Gateway, Lambda, DynamoDB).
* Design DynamoDB database using Single Table Design model (Access Patterns, PK/SK Design).
* Design UI/UX for main functional pages: Dashboard, Resume Builder, Mock Interview.
* Initialize Monorepo project structure (Backend Java Spring Cloud & Frontend React Vite).
* Set up Infrastructure as Code (IaC) with AWS SAM (template.yaml) and configure IAM/Bedrock permissions.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | --- | :---: | :---: | --- |
| 2 | - Draw Data Flow diagram: User -> CloudFront -> API Gateway -> Lambda.<br>- Identify necessary AWS services: Bedrock (Claude 3), Cognito, DynamoDB.<br>- Analyze business requirements for 4 main modules: Resume, Cover Letter, Interview, Industry Insight.<br>- List necessary API Endpoints. | 17/11/2025 | 17/11/2025 |  |
| 3 | - Define Entities: User, Resume, Assessment, CoverLetter.<br>- Determine Access Patterns (Data querying methods).<br>- Design Partition Key (PK) and Sort Key (SK) for each Entity (e.g., USER#{id}, METADATA, RESUME, LETTER#uuid).<br>- Design Global Secondary Index (GSI) if needed. | 18/11/2025 | 18/11/2025 | |
| 4 | - Design login/registration flow with Cognito.<br>- Sketch statistical Dashboard interface.<br>- Design Resume Builder interface (split screen: Form & Preview).<br>- Design Mock Interview interface (Quiz form).<br>- Select UI library set: Tailwind CSS, Shadcn UI, Lucide Icons. | 19/11/2025 | 19/11/2025 |  |
| 5 | - Setup environment: Java 17, Maven, Node.js, AWS CLI, SAM CLI.<br>- Initialize Backend structure: Spring Boot 3 + Spring Cloud Function.<br>- Initialize Frontend structure: React + Vite.<br>- Configure Git Repository and Monorepo directory structure. | 20/11/2025 | 20/11/2025 | |
| 6 | - Create template.yaml file.<br>- Declare DynamoDB Table resources (PAY_PER_REQUEST).<br>- Declare Cognito User Pool & Client.<br>- Declare S3 Bucket & CloudFront Origin Access Control (OAC).<br>- Configure IAM Policies for Lambda (permissions to call Bedrock & DynamoDB).<br>- Register Model Access (Claude 3 Haiku) on AWS Bedrock. | 21/11/2025 | 21/11/2025 |  |

### Week 11 Achievements:
* Complete Serverless system architecture design.
* DynamoDB Single Table design schema ready for coding.
* Development Environment fully installed.
* AWS SAM template.yaml file ready to deploy basic infrastructure.
* Access rights to AI Claude 3 model on AWS Bedrock activated.