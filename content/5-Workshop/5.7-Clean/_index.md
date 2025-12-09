---
title : "Clean up"
#date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

After completing the workshop, it is extremely important to delete the created resources to avoid unexpected AWS charges at the end of the month.

The cleanup process consists of 2 main steps.

## Step 1: Delete Frontend Bucket (Required) 🗑️

The `sam delete` command will **fail** when trying to delete an S3 Bucket if that bucket still contains files (the frontend code you uploaded). Therefore, we need to manually delete this bucket first.

1.  Open Terminal.
2.  Run the following command to remove all files and delete the bucket (replace with your bucket name):

```bash
aws s3 rb s3://career-coach-frontend-123456789 --force
```

Explanation:

* rb: Remove Bucket
* --force: Force deletion even if the bucket still contains files (dangerous but convenient for workshops).

## Step 2: Delete Backend Stack (SAM Delete)

Once the Bucket is deleted, we can now safely delete the entire remaining infrastructure (Lambda, DynamoDB, API Gateway, Cognito...).

At the project root directory (where the template.yaml file is located), run the command:

```bash
sam delete
```

Confirm action:

* Are you sure you want to delete the stack [career-coach-stack]? -> Enter y (Yes).
* Are you sure you want to delete the folder [career-coach-stack]...? -> Enter y.

This process will take about 2-3 minutes. SAM will clean up everything defined in `template.yaml`.