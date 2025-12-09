---
title : "Deploy"
#date : "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

We have completed writing the code (Dev) and designing the infrastructure (Ops). Now it is time to combine them (DevOps) to bring the application to the real world.

The deployment process consists of 3 main stages:
1.  **Backend Deployment:** Push API and Database to AWS.
2.  **Frontend Configuration:** Update connection information (API URL, Cognito ID) in the React code.
3.  **Frontend Deployment:** Build and push the Static Web to S3.

---

## Stage 1: Backend Deployment (AWS SAM)

### Step 1: Build Java source code

Before deploying, we need to package the Java source code into an executable `.jar` file.

Open Terminal at the project root directory (where the parent `pom.xml` file or `backend/` directory is located):

```bash
cd backend
mvn clean package -DskipTests
```

Explanation:

* clean: Removes old build files.
* package: Packages into a JAR file (located in target/).
* -DskipTests: Skips running unit tests to save time for the workshop.

### Step 2: Deploy to AWS

Return to the root directory (where template.yaml is located) and run the command:

```bash
cd ..
sam deploy --guided
```

Follow the on-screen instructions (just press Enter to select defaults for most questions):

* Stack Name: career-coach-stack (or any name you prefer).
* AWS Region: ap-southeast-1 (Singapore).
* Confirm changes before deploy: Y.
* Allow SAM CLI IAM role creation: Y.
* Save arguments to configuration file: Y.

This process will take about 5-7 minutes for CloudFormation to create the Database, Lambda, API Gateway, and CloudFront.

### Step 3: Save Output Information

After successful deployment, the screen will display the Outputs table. Copy the following parameters to Notepad; we will need them immediately:

* ApiEndpoint: API Gateway URL (Example: https://xyz.execute-api...).
* CognitoUserPoolId: User Pool ID.
* CognitoClientId: App Client ID.
* S3BucketName: Bucket name for hosting the web.
* WebsiteURL: Website URL (CloudFront).

## Stage 2: Frontend Configuration

The Frontend needs to know which API to call and where to log in.

### Step 1: Update API Client

Open the file `frontend/src/lib/api.js`:

```javaScript

// Thay thế bằng ApiEndpoint bạn vừa copy
const BASE_URL = "https://xxxxxxxxx.execute-api.ap-southeast-1.amazonaws.com"; 
```
### Step 2: Update Auth Config

Open the file `frontend/src/main.jsx`:

```javaScript

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: 'ap-southeast-1_XXXXXXX', // Paste CognitoUserPoolId
      userPoolClientId: 'xxxxxxxxxxxxxxxxx',  // Paste CognitoClientId
      loginWith: { email: true }
    }
  }
});
```
## Stage 3: Frontend Deployment (S3 & CloudFront)

### Step 1: Build React project (Vite)

Navigate to the frontend directory and proceed to build:

```bash
cd frontend
npm run build
```

Result: A folder named `dist` will be created. This is the "lean" version of the website, which has been size-optimized.

### Step 2: Push code to S3

Use AWS CLI to sync the `dist` folder to the S3 Bucket (use the bucket name you saved in Stage 1):

```Bash
aws s3 sync dist s3://career-coach-frontend-123456789 --delete
``` 

The `--delete` parameter: Automatically deletes old files on S3 if they no longer exist in the `dist` directory (helps with cleanup).

### Step 3: Clear CloudFront Cache (Invalidation)

This is a mandatory step every time you update the frontend code. If you skip this step, users will still see the old web version due to CloudFront caching.

Get the CloudFront Distribution ID:

```bash
aws cloudfront list-distributions --query "DistributionList.Items[*].{ID:Id, Domain:DomainName}" --output table
```

Run the cache deletion command:

```bash
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths
```