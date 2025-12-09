---
title: "Worklog Week 12"
#date: "`r Sys.Date()`"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Week 12 Objectives:
* Build and finalize 5 Lambda Functions using Java Spring Cloud Function.
* Implement Repository layer interacting with DynamoDB (Enhanced Client).
* Integrate AWS SDK v2 to call Amazon Bedrock (Claude 3 Haiku) for AI features.
* Handle advanced technical issues: Prompt Engineering, JSON Serialization (ObjectMapper), Error Handling.
* Deploy Backend to AWS and test API via Postman.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | --- | :---: | :---: | --- |
| 2 | - Create UserEntity & UserRepository.<br>- Write CRUD logic for user information.<br>- Integrate JWT authentication logic from Cognito in Lambda.<br>- Handle Onboarding logic for new users.<br>- Deploy first test Lambda with SAM. | 24/11/2025 | 24/11/2025 | |
| 3 | - Create ResumeEntity & ResumeRepository.<br>- Write logic to save/retrieve Markdown content from DynamoDB.<br>- Manually configure ObjectMapper to handle JSON Serialization errors (avoid empty object errors).<br>- Write Unit Tests for Service layer. | 25/11/2025 | 25/11/2025 |  |
| 4 | - Configure BedrockRuntimeClient in Java.<br>- Write improveWithAI Service: Send raw text -> Receive improved text.<br>- Perform Prompt Engineering to optimize results from Claude 3.<br>- Handle Exceptions and Timeouts when calling AI. | 26/11/2025 | 26/11/2025 |  |
| 5 | - CoverLetter: Write Prompt to create cover letter based on JD and Profile.<br>- Interview: Write logic to generate multiple-choice questions (Generate Quiz).<br>- Interview: Handle Prompt to ensure AI returns correct JSON Array format.<br>- Interview: Write logic to save scores (Save Result) into DynamoDB. | 27/11/2025 | 27/11/2025 |  |
| 6 | - Build IndustryInsightFunction (Optional - Cache strategy).<br>- Review and Refactor code (Clean Code).<br>- Build entire project (`mvn clean package`).<br>- Run `sam deploy` to push entire Backend to AWS.<br>- Test all API Endpoints via Postman/Curl. | 28/11/2025 | 28/11/2025 |  |

### Week 12 Achievements:
* 5 Lambda Functions operating stably on AWS.
* API Gateway has exposed necessary endpoints.
* Successfully integrated Amazon Bedrock to generate CV content, Cover Letter, and Quiz.
* Data stored and retrieved accurately from DynamoDB.
* Resolved errors related to JSON Serialization in Serverless environment.