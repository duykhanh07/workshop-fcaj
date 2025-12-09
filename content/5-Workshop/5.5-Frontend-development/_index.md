---
title : "Frontend Development"
#date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

The **AI Career Coach** Frontend not only needs to be beautiful but also must handle complex logic like a Markdown Editor, Quiz Game, and secure API calls.

View the full Frontend source code [here](https://github.com/duykhanh07/ai-career-coach/tree/main/frontend)

Below I only present the key points and what to keep in mind when building the frontend.

## 1. Project Initialization (Tech Stack Setup)

We use **Vite** to initialize the React project because of its extremely fast build speed.

### Main libraries:
* **Tailwind CSS:** A utility-first CSS Framework.
* **Shadcn UI:** A beautiful component library (Button, Input, Card, Accordion...) that helps us avoid coding CSS from scratch.
* **AWS Amplify:** A library that helps connect React with Cognito easily.
* **React Router Dom:** To navigate between pages.

```bash
npm create vite@latest frontend -- --template react
cd frontend
npm install
```

**Libraries to install**

```bash
npm install react-router-dom aws-amplify @aws-amplify/ui-react lucide-react react-hook-form zod @hookform/resolvers sonner @uiw/react-md-editor recharts html2pdf.js react-spinners date-fns clsx tailwind-merge class-variance-authority
```
## 2. Authentication Configuration with AWS Amplify

Instead of manually writing complex Login/Register pages, we use Amplify's `<Authenticator>` component. It automatically generates a fully functional login/registration interface.

Configuration in main.jsx: You need to get the UserPoolId and ClientId from the Output section of the sam deploy command in the previous lesson to fill in here.

```javaScript

import { Amplify } from 'aws-amplify';

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: 'ap-southeast-1_XXXXXXX', // <-- Thay bằng ID của bạn
      userPoolClientId: 'xxxxxxxxxxxxxxxxx',  // <-- Thay bằng Client ID của bạn
      loginWith: { email: true }
    }
  }
});
```
![login](/images/5-Workshop/5.5-Frontend-development/login.jpeg)

## 3. apiClient
This is the most critical part of the integration.

**The Problem:**

When the Java Backend returns data, it sometimes returns a JSON string nested within another JSON string (Double Serialization).
* Example Backend response: `{ statusCode: 200, body: "{\"pk\": \"USER#1\", ...}" }`
* If the Frontend simply uses `response.json()`, it will receive a string inside the body and cannot access `data.pk`.

**Solution: `apiClient` Wrapper**

We write a wrapper function to:

1. Automatically retrieve the JWT Token from the current session.
2. Attach the Token to the Authorization Header.
3. Automatically parse JSON twice if nested data is detected.

```javaScript

// src/lib/api.js
export const apiClient = async (endpoint, options = {}) => {
    // 1. Lấy Token
    const session = await fetchAuthSession();
    const token = session.tokens?.idToken?.toString();

    // 2. Gọi API với Token
    const response = await fetch(`${BASE_URL}${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${token}`,
        'Content-Type': 'application/json',
      },
      ...options,
    });

    // 3. Xử lý "Double JSON"
    const data = await response.json();
    if (data.body && typeof data.body === 'string') {
        return JSON.parse(data.body); // <-- "Bóc vỏ" lần 2
    }
    return data;
};
```
## 4. Building Feature Pages (Core Features)
### A. Dashboard

The overview page displaying statistics.

* Challenge: Handling asynchronicity when calling the API to fetch activity history.
* Solution: Use `useEffect` to fetch data and display `BarLoader` (loading spinner) while waiting.

![dashboard](/images/5-Workshop/5.5-Frontend-development/dashboard.png)

### B. Resume Builder

This is the most complex feature in terms of UI.

* Interface: Use Tabs to switch between Edit Form and Markdown Preview modes.
* Form: Use `react-hook-form` and `zod` to validate input data.
* AI Feature: When the user clicks "Improve with AI", the Frontend sends the raw text to the `/resume/improve` API and populates the input field with the result.
* Accordion UI: Use Shadcn's Accordion component to group Experience/Education sections neatly.

![resume builder form](/images/5-Workshop/5.5-Frontend-development/resume-builder-form.jpeg)

![resume builder markdown](/images/5-Workshop/5.5-Frontend-development/resume-builder-markdown.jpeg)

### C. Mock Interview

Knowledge quiz page.

* Scoring Logic: Compare the answer selected by the user with the correct answer from the Backend.
* Technical Note: The Backend returns the correct answer as the character "B", but the Frontend displays "B. Content...".
* Processing Code: `userAnswer.split(".")[0].trim() === correctAnswer`.

![quiz](/images/5-Workshop/5.5-Frontend-development/quiz.jpeg)

![quiz result](/images/5-Workshop/5.5-Frontend-development/quiz-result.jpeg)

### D. Cover Letter Generator

This feature helps users generate cover letters automatically based on the Job Description (JD).

* **Process Flow:**
    1.  User enters: *Company Name*, *Position*, *Job Description (JD)*.
    2.  Frontend sends `POST /cover-letters` to Backend.
    3.  Backend calls Claude 3 to write the letter and saves it to DB.
    4.  Frontend receives the result and displays it to the user.

* **Technical Note:**
    * **Loading State:** Since AI takes about 5-10 seconds to write a letter, the Frontend **must** have a waiting state (Loading Spinner or Skeleton) so users know the system is processing, preventing them from clicking the button multiple times.
    * **Result Display:** The returned content is plain text (or Markdown), which needs to be displayed in a `textarea` or `MDEditor` so users can edit it before downloading.

![cover letter form](/images/5-Workshop/5.5-Frontend-development/cover-letter-form.jpeg)
