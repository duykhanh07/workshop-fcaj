---
title : "Prerequiste"
#date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

To build and deploy the **AI Career Coach** application, we need to set up a robust development environment. Please ensure you install all the tools below before diving into the code.

{{% notice note %}}
ℹ️ **Important Note:** This project requires you to have an AWS (Amazon Web Services) account. If you don't have one, please sign up for an account [here](https://aws.amazon.com/free/). A Free Tier account is sufficient to complete this workshop.
{{% /notice %}}

---

## 1. Install Runtime (Programming Language)

### A. Java 17 (JDK)

Our Backend uses Spring Boot 3, which requires a minimum of Java 17.

1.  **Download:** Visit [Oracle JDK 17](https://www.oracle.com/java/technologies/downloads/#java17) or [Amazon Corretto 17](https://aws.amazon.com/corretto/).
2.  **Install:** Run the installer file for your operating system (Windows/Mac/Linux).
3.  **Verify:** Open Terminal (or CMD/PowerShell) and type the command:

```bash
java -version
```
Expected Result: You will see version 17.x.x displayed.

### B. Node.js & npm
The React Frontend (Vite) requires a Node.js environment.

1. **Download:** Visit Node.js and download the LTS (Long Term Support) version (e.g., v18 or v20).
2. **Verify:** Open Terminal (or CMD/PowerShell) and type the command:
```bash
node -v
npm -v
```

---

## 2. Install AWS Tools
These are the tools that help us interact with AWS Cloud and deploy Serverless applications.

### A. AWS CLI (Command Line Interface)
The official AWS command line tool.

* Windows: Download the MSI installer [here.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* MacOS: Download the PKG file [here.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* Linux: See detailed instructions [here.](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Verify installation:

```bash
aws --version
```
### B. AWS SAM CLI (Serverless Application Model)
A tool that makes it easier to build, test, and deploy Serverless applications (Lambda, DynamoDB...).

* Installation instructions: [See official AWS SAM documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
* Verify installation:
```
sam --version
```

---

## 3. Configure AWS Credentials
After installing AWS CLI, you need to connect it to your AWS account via an Access Key.

**Step 1: Create Access Key**
* Log in to the AWS Console.
* Find the IAM service -> Select Users -> Select your User (or create a new one).
* Go to the Security credentials tab -> Click Create access key.
* Select Use case: Command Line Interface (CLI).
* Download the .csv file containing the Access Key ID and Secret Access Key to your machine.

**Step 2: Configure on machine**

Open Terminal and run the command:

```
aws configure
```
Enter the following information:

* AWS Access Key ID: [Paste your Key ID]
* AWS Secret Access Key: [Paste your Secret Key]
* Default region name: ap-southeast-1 (Singapore) (Or the region closest to you)
* Default output format: json

---

## 4. Activate AI Model (Important)
The project uses Anthropic's Claude 3 Haiku model via Amazon Bedrock. By default, this access is disabled.

1. Log in to the AWS Console, search for the Amazon Bedrock service.
2. Ensure you are in the configured Region (e.g., Singapore).
3. In the left menu, select Model catalog.
4. Find the provider Anthropic.
{{% notice note  %}}
ℹ️ **Note:** You need to submit the "Use Case Details" form first. Enter "Personal Project" for the purpose of learning.
{{% /notice %}}
5. Check Claude 3 Haiku.
6. If **Open in playground** is orange, it is ready to use.

![claude 3](/images/5-Workshop/5.2-Prerequisite/claude-3.png)

---

## 5. IDE & Extensions (Recommended)
To code most efficiently, you should use Visual Studio Code (VS Code) or IntelliJ IDEA.

If using VS Code, install the following Extensions:

* Extension Pack for Java: Supports Java/Spring Boot coding.
* ES7+ React/Redux/React-Native snippets: Supports fast React coding.
* AWS Toolkit: Manage AWS resources right inside VS Code.
* YAML: Supports syntax highlighting for the template.yaml file.

---

✅ Verification Checklist
Before moving to the next lesson, make sure you have checked off the following items:
* Installed Java 17 (`java -version`).
* Installed Node.js (`node -v`).
* Installed AWS SAM CLI (`sam --version`).
* Ran aws configure successfully.
* Saw Open in playground for Claude 3 Haiku on AWS Bedrock.
