---
title: "Worklog Week 5"
#date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives:
* Get familiar with system scaling concepts (Load Balancer, Auto Scaling) and know how to practically deploy on AWS.
* Grasp the process of deploying applications and databases using RDS, managing machine images (AMIs), and creating Launch Templates.
* Understand and apply CloudWatch to monitor resources, track logs, create metrics, alarms, and dashboards.
* Learn the basics of Route 53, DNS resolver, and integration with Microsoft AD in a cloud environment.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Practice <br> &emsp; + Deploy Akaunting Instance <br> &emsp;&emsp; + Create instance <br> &emsp;&emsp; + Configure Networking <br> &emsp;&emsp; + Deploy Prestashop E-Commerce <br> &emsp; + Secure Wordpress application <br> &emsp; + Create Snapshot <br> &emsp; + Migrate to a larger instance <br> &emsp; + Create alarms to notify administrators if a specific event occurs <br> - Learn about Auto Scaling Group, AMIs, and Launch Template <br> - Practice <br> &emsp; + Set up basic network infrastructure for FCJ Management application <br> &emsp;&emsp; + VPC and Subnet <br> &emsp;&emsp; + Internet Gateway <br> &emsp;&emsp; + Route Table <br> &emsp;&emsp; + Security Groups <br> &emsp; + Create EC2 Instance <br> &emsp; + Create DB Subnet Group for Amazon RDS <br> &emsp; + Create Amazon RDS Database Instance <br> &emsp; + Install data for Database <br> &emsp; + Deploy web server <br> &emsp; + Prepare data for Predictive scaling <br> &emsp; + Create Amazon Machine Images (AMIs) from EC2 <br> &emsp; + Create Launch Templates | 06/10/2025 | 06/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Learn about Elastic Load Balancing (ELB) <br> &emsp; + Target Group <br> &emsp; + Application Load Balancer <br> &emsp; + Network Load Balancer <br> &emsp; + Classic Load Balancer <br> &emsp; + Gateway Load Balancer <br> - Learn about Scaling solutions / techniques <br> &emsp; + Manual scaling <br> &emsp; + Scheduled scaling <br> &emsp; + Dynamic scaling <br> &emsp; + Predictive scaling <br> - Practice: <br> &emsp; + Create Target Group <br> &emsp; + Create Application Load Balancer <br> &emsp; + Check Elastic Load Balancing (ELB) deployment results <br> &emsp; + Create Auto Scaling Group <br> &emsp; + Connect ASG with Load Balancer <br> &emsp; + Set up Size and Scaling Policies <br> &emsp; + Set up notifications <br> - Practice Scaling solutions / techniques <br> &emsp; + Manual scaling <br> &emsp; + Scheduled scaling <br> &emsp; + Dynamic scaling <br> &emsp; + Predictive scaling | 07/10/2025 | 07/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and AWS Study Group Youtube |
| 4 | - Learn about AWS CloudWatch <br> &emsp; + Features <br> &emsp; + Container Insights <br> &emsp; + Key benefits <br> - Learn about CloudWatch Metrics, CloudWatch Logs, CloudWatch Alarms, CloudWatch Dashboards | 08/10/2025 | 08/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5 | - Practice: <br> &emsp; + Deploy CloudFormation Stack <br> &emsp; + View Metrics <br> &emsp; + Interact with charts in CloudWatch <br> &emsp; + Perform Metrics searches <br> &emsp; + Perform math operations <br> &emsp; + Create dynamic labels <br> &emsp; + View CloudWatch Logs <br> &emsp; + Generate logs from an application, then query these logs in CloudWatch Logs Insights <br> &emsp; + Extract valuable data from logs and convert them into metrics using CloudWatch Metric Filters <br> &emsp; + Set up Alarm for Error Log Metric <br> &emsp; + Create a simple Dashboard to centrally manage Metrics and Alarms | 09/10/2025 | 10/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Learn about Route 53 <br> &emsp; + Outbound Endpoints <br> &emsp; + Inbound Endpoints <br> &emsp; + Route 53 Resolver Rules <br> - Learn about AWS Quick Starts, AWS CloudFormation, AWS Directory Service <br> - Practice: <br> &emsp; + Create Key Pair, initialize CloudFormation Template, configure Security Group <br> &emsp; + Connect to RDGW <br> &emsp; + Deploy Microsoft AD <br> &emsp; + Configure DNS <br> &emsp;&emsp; + Create Route 53 Outbound Endpoint <br> &emsp;&emsp; + Create Route 53 Resolver Rules <br> &emsp;&emsp; + Create Route 53 Inbound Endpoints <br> &emsp;&emsp; + Test results | 10/10/2025 | 10/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 5 Achievements:
* Deploy and manage web applications on EC2/Lightsail, including Akaunting, WordPress, and Prestashop, along with security configuration, snapshots, and instance upgrades.
* Set up Auto Scaling Group, Launch Template, and AMI to scale the system, applying scaling methods: manual, scheduled, dynamic, predictive.
* Create and manage Load Balancers (ALB, NLB) connected to Auto Scaling Group to balance load and ensure system stability.
* Monitor the system using CloudWatch: Metrics, Logs, Alarms, Dashboards, and extract data from logs to track performance.
* Deploy database with Amazon RDS, set up DB Subnet Group, and connect EC2 with RDS for complete application operation.
* Learn and practice Route 53, deploy internal DNS, Microsoft AD, configure Resolver Rules, and check connections.