---
title: "Worklog Week 6"
#date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives:
* Understand the process of building AMI, Launch Template and deploying WordPress in a scalable environment.
* Use Lambda Function to optimize costs, automatically start/stop EC2 Instances according to demand.
* Get familiar with Grafana to monitor data and systems.
* Know how to manage resources using Tags, Resource Groups and apply access control using IAM.
* Learn about AWS Systems Manager to run remote commands and manage patches (Patch Manager).

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Practice: deploy Wordpress application with Auto Scaling Group to ensure the application's scalability according to visitor demand <br> &emsp; + Prepare VPC and subnet, create Security Group for EC2, Database and initialize EC2, Database <br> &emsp; + Install wordpress on EC2 <br> &emsp; + Perform Autoscaling creation for wordpress Instance <br> &emsp;&emsp; + Initialize AMI from Webserver Instance <br> &emsp;&emsp; + Initialize Launch Template <br> &emsp;&emsp; + Initialize Target Group <br> &emsp;&emsp; + Initialize Load Balancer <br> &emsp;&emsp; + Initialize Auto Scaling Group <br> &emsp; + Backup and restore database <br> &emsp; + Initialize Cloudfront for Web Server | 13/10/2025 | 13/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Learn about Lambda function <br> - Practice: use Lambda function to optimize costs for your system on AWS environment <br> &emsp; + Create VPC, Security Group, EC2 <br> &emsp; + Incoming Web-hooks slack <br> &emsp; + Create tag for instance <br> &emsp; + Create Role for Lambda <br> &emsp; + Create Lambda Function performing Stop instances function <br> &emsp; + Create Lambda Function performing Start instances function <br> &emsp; + Check results | 14/10/2025 | 14/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4 | - Learn about Grafana <br> - Practice: <br> &emsp; + Create VPC and Subnet, Security Group, Linux EC2 Instance <br> &emsp; + Create IAM User, IAM Role, assign IAM Role to EC2 Instance <br> &emsp; + Install Grafana <br> &emsp; + Perform EC2 connection using MobaXterm <br> &emsp; + Monitor with Grafana | 15/10/2025 | 15/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and [grafana.com/grafana](https://grafana.com/grafana) |
| 5 | - Learn about resource management using Tag and Resource Groups <br> - Practice: <br> &emsp; + Create EC2 Instance with tag <br> &emsp; + Add or remove tags on individual resources and on resource groups <br> &emsp; + Filter resources by tag <br> &emsp; + Create a Resource Group <br> - Practice: manage access to EC2 Resource Tag service with AWS IAM <br> &emsp; + Create IAM User <br> &emsp; + Create IAM Policy <br> &emsp; + Create IAM Role <br> &emsp; + Switch Role <br> &emsp; + Check IAM Policy <br> &emsp;&emsp; + Proceed to access EC2 console in AWS Region - Tokyo <br> &emsp;&emsp; + Proceed to access EC2 console in AWS Region - North Virginia <br> &emsp;&emsp; + Proceed to create EC2 instance when there are no and there are Tags satisfying conditions <br> &emsp;&emsp; + Edit Resource Tag on EC2 Instance <br> &emsp;&emsp; + Check policy | 16/10/2025 | 16/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Learn about AWS Systems Manager <br> - Practice: manage Patch and run commands on multiple servers with AWS System Manager <br> &emsp; + Create VPC, Subnet, EC2 Instance, IAM Role and assign IAM Role <br> &emsp; + Set up Patch Manager <br> &emsp; + Run Command | 17/10/2025 | 17/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 6 Achievements:
* Deploy WordPress according to scalable model including: create VPC/Subnet/SG, install Webserver, create AMI, Launch Template, Target Group, Load Balancer and Auto Scaling Group; simultaneously backup – restore database and configure CloudFront.
* Build cost optimization automation solution using Lambda, including create Role, assign tag, create Stop/Start EC2 Lambda Function and check activation via Slack Webhook.
* Deploy Grafana to monitor the system, including create Linux EC2, IAM User/Role, install Grafana and connect to track data.
* Manage resources using Tag and Resource Groups, simultaneously apply IAM to control access by Tag and test in multiple Regions.
* Use AWS Systems Manager to manage patches using Patch Manager and run remote commands with Run Command.