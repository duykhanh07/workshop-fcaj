---
title: "Worklog Week 13"
#date: "`r Sys.Date()`"
weight: 13
chapter: false
pre: " <b> 1.13. </b> "
---
### Week 13 Objectives:
* Build complete React Frontend with Vite, Tailwind CSS, and Shadcn UI.
* Integrate AWS Amplify to handle Login/Registration.
* Connect Frontend with Backend API Gateway (Integration).
* Finalize core features: Resume Builder, Quiz Game, Dashboard.
* Build, Deploy Frontend to S3/CloudFront and test the entire system (UAT).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| :---: | --- | :---: | :---: | --- |
| 2 | - Install Tailwind CSS, Shadcn UI components (Button, Card, Input...).<br>- Configure AWS Amplify (Amplify.configure) with UserPoolId from Backend.<br>- Create Login/Register page using Authenticator component.<br>- Create Main Layout (Sidebar, Header, Protected Routes). | 01/12/2025 | 01/12/2025 | AWS Amplify UI Documentation |
| 3 | - Code Dashboard page: Fetch history data and display statistics.<br>- Code Resume Builder page: Integrate Markdown Editor (@uiw/react-md-editor).<br>- Build CV input Form with React Hook Form & Zod Validation.<br>- Integrate PDF export feature (html2pdf.js). | 02/12/2025 | 02/12/2025 | React Hook Form Documentation |
| 4 | - Write `apiClient` wrapper: Automatically attach Token and handle "Double JSON parsing".<br>- Connect "Improve with AI" button in Resume Builder with Backend API.<br>- Build Cover Letter Generator interface: JD Input Form -> Display AI result.<br>- Handle loading state (Skeleton/Spinner) while waiting for AI response. | 03/12/2025 | 03/12/2025 | |
| 5 | - Code quiz interface (Radio buttons).<br>- Handle scoring logic and display correct/incorrect results (String comparison logic).<br>- Draw Performance Chart using Recharts.<br>- Review overall interface, fix CSS errors, Mobile Responsive. | 04/12/2025 | 04/12/2025 |  |
| 6 | - Run `npm run build` to package Frontend.<br>- Sync code to S3 (`aws s3 sync`).<br>- Invalidate CloudFront Cache to update latest code.<br>- Perform End-to-End Testing on Production environment. | 05/12/2025 | 05/12/2025 |  |

### Week 13 Achievements:
* Complete AI Career Coach website, operating smoothly on the Internet environment.
* Users can log in, create CV, write Cover Letter, and practice interviewing.
* Frontend successfully connected with Serverless Backend and AI Bedrock.
* System partially deployed automatically via CLI.
* Ready for demo.