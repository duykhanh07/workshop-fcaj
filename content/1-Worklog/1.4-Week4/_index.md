---
title: "Worklog Week 4"
#date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Week 4 Objectives:

* Understand and practice AWS core storage and content delivery services including S3, CloudFront, Cloud9, RDS, and Lightsail.  
* Learn how to deploy, manage, and secure data in Amazon S3 (including Website Hosting, Versioning, and Cross-Region Replication).  
* Understand how to deploy static and dynamic web applications using S3 + CloudFront + RDS + EC2.  
* Get familiar with the AWS Cloud9 cloud development environment and understand the AWS Well-Architected Framework for optimal system design.  
* Learn how to quickly deploy open-source applications (WordPress, PrestaShop) using Amazon Lightsail.  

### Tasks for this week:

| Day | Task | Start Date | End Date | Reference |
| --- | ---- | ----------- | -------- | ---------- |
| 2 | - Study the basics of AWS Cloud9 and practice: <br>&emsp; + Create a Cloud9 instance <br>&emsp; + Use the command line <br>&emsp; + Work with text files <br>&emsp; + Use AWS CLI on AWS Cloud9 <br> - Learn the basics of AWS S3 <br>&emsp; + Concept and purpose of Object Storage <br>&emsp; + Structure of Bucket and Object <br>&emsp; + Common Storage Classes <br>&emsp; + Manage access control and data sharing (Public Access, Bucket Policy) <br> - **Practice:** <br>&emsp; + Create an S3 Bucket <br>&emsp; + Upload data to S3 Bucket <br>&emsp; + Enable Static Website Hosting <br>&emsp; + Configure Block Public Access <br>&emsp; + Configure public object access <br>&emsp; + Verify hosted Website | 29/09/2025 | 29/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Study the basics of Amazon CloudFront <br>&emsp; + Understand CDN concepts and CloudFront’s role in content delivery <br>&emsp; + Basic structure: Origin, Distribution, Edge Location <br>&emsp; + How CloudFront accelerates S3 content delivery <br> - Learn about S3 Versioning <br>&emsp; + Key benefits of Versioning <br>&emsp; + How Versioning works <br>&emsp; + Versioning states <br> - **Practice:** <br>&emsp; + Block all public access to S3 <br>&emsp; + Create CloudFront Distribution serving an S3 Bucket <br>&emsp; + Test Amazon CloudFront <br>&emsp; + Enable Versioning for the Bucket and update content <br>&emsp; + Test Versioning on S3 and CloudFront | 30/09/2025 | 30/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and [aws.amazon.com/cloudfront](https://aws.amazon.com/cloudfront/) |
| 4 | - Learn about Amazon S3 Cross-Region Replication (CRR): <br>&emsp; + Key benefits <br>&emsp; + How CRR works <br> - Study the AWS Well-Architected Framework: <br>&emsp; + Concept and six pillars of AWS Well-Architected Framework <br>&emsp; + Well-Architected Lenses <br> - **Practice:** <br>&emsp; + Move objects to a new Bucket <br>&emsp; + Replicate S3 objects to another Region <br> - Study Best Practices & Troubleshooting for AWS S3: <br>&emsp; + Security <br>&emsp; + Performance <br>&emsp; + Cost optimization <br>&emsp; + Common issues and resolutions | 01/10/2025 | 01/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and [aws.amazon.com/architecture/well-architected](https://aws.amazon.com/architecture/well-architected/) |
| 5 | - Study the basics of Amazon RDS: <br>&emsp; + Introduction to Relational Database Service <br>&emsp; + Supported database engines (MySQL, PostgreSQL, MariaDB, etc.) <br>&emsp; + Basic structure: DB Instance, Subnet Group, Security Group <br> - **Practice:** <br>&emsp; + Create VPC and Security Groups for EC2 and RDS <br>&emsp; + Create a DB Subnet Group <br>&emsp; + Launch an EC2 instance <br>&emsp; + Create an RDS database instance <br>&emsp; + Deploy an application <br>&emsp; + Perform Backup and Restore in Amazon RDS | 02/10/2025 | 03/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - **Practice:** Deploy open-source applications on Amazon Lightsail and configure resources: <br>&emsp; + Deploy a database on Lightsail <br>&emsp; + Deploy a WordPress server <br>&emsp; + Configure Ubuntu <br>&emsp; + Configure Networking <br>&emsp; + Configure WordPress <br>&emsp; + Deploy PrestaShop E-Commerce instance | 03/10/2025 | 03/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 4 Achievements:

* Became familiar with AWS Cloud9, used the CLI, and managed resources through the cloud development environment.  
* Gained a solid understanding of Amazon S3 — created and configured Buckets, hosted a static website, enabled Block Public Access and Versioning.  
* Understood global content delivery with Amazon CloudFront and integrated it with S3 to accelerate and secure websites.  
* Learned and practiced S3 Cross-Region Replication (CRR), applying best practices for security, performance, and cost optimization.  
* Deployed and connected Amazon RDS with EC2 instances, performed database Backup and Restore successfully.  
* Studied the AWS Well-Architected Framework and understood its six architectural pillars.  
* Successfully deployed and configured WordPress and PrestaShop applications on Amazon Lightsail, mastering resource management and basic security.  
