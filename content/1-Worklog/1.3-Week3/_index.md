---
title: "Worklog Week 3"
#date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Week 3 Objectives:

* Understand and practice advanced configurations of VPN and EC2 on AWS.  
* Learn how to set up advanced VPN with StrongSwan, Transit Gateway, and BGP Dynamic Routing to extend network connectivity between regions and systems.  
* Master advanced EC2 management and operation: changing instance types, creating custom AMIs, and recovering access when losing a Key Pair.  
* Deploy real-world applications (Node.js and User Management) on Amazon Linux 2 and Windows Server 2022.  
* Manage access control and resource usage limits with IAM Policy to enhance system security.  

### Tasks to Implement This Week:

| Day | Task | Start Date | Completion Date | Reference |
| --- | ---- | ----------- | ---------------- | ---------- |
| 2 | - Study advanced VPN configuration basics: <br>&emsp; + StrongSwan replacement setup <br>&emsp; + Advanced security configuration <br>&emsp; + BGP Dynamic Routing setup <br>&emsp; + Advanced monitoring and troubleshooting <br>&emsp; + Production deployment checklist <br> - Explore common VPN setup issues and solutions for different OS environments <br> - **Practice:** Configure VPN using StrongSwan with Transit Gateway <br>&emsp; + Create Customer Gateway <br>&emsp; + Create Transit Gateway <br>&emsp; + Create VPN connection <br>&emsp; + Create Transit Gateway Attachment <br>&emsp; + Configure Route Tables for Transit Gateway and VPC <br>&emsp; + Configure Customer Gateway | 22/09/2025 | 22/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Review EC2 fundamentals: <br>&emsp; + Concepts, features, and how EC2 works <br>&emsp; + Instance types (General Purpose, Compute Optimized, Memory Optimized, etc.) <br>&emsp; + AMI, EBS, Security Group, Key Pair <br>&emsp; + Network, storage, and access configurations <br>&emsp; + Lifecycle management (Start, Stop, Reboot, Terminate) <br> - **Practice:** <br>&emsp; + Create VPC and Security Groups for Linux and Windows <br>&emsp; + Launch a Microsoft Windows Server 2022 instance <br>&emsp; + Connect from local PC to Windows Server 2022 instance <br>&emsp; + Launch an Amazon Linux 2 instance <br>&emsp; + Connect to Amazon Linux 2 instance using MobaXterm and PuTTY | 23/09/2025 | 23/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4 | - **Practice:** Advanced work with Amazon EC2 and related components <br>&emsp; + Change EC2 instance type <br>&emsp; + Create EC2 Snapshot <br>&emsp; + Create custom AMI <br>&emsp; + Launch new instance from custom AMI <br>&emsp; + Recover access when Key Pair is lost (Linux - User Data, Windows - SSM) <br> - Research: <br>&emsp; + Desktop Interface for EC2 Ubuntu 22.04 <br>&emsp; + EBS Snapshots Archive <br>&emsp; + AMI Sharing | 24/09/2025 | 24/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5 | - **Practice:** Deploy a Node.js application on Amazon Linux 2 <br>&emsp; + Prepare LAMP Web Server <br>&emsp; + Verify LAMP Web Server <br>&emsp; + Configure database server security <br>&emsp; + Install phpMyAdmin <br>&emsp; + Deploy application on Linux instance <br> - **Practice:** Deploy AWS User Management application on Amazon EC2 (Windows Server 2022) <br>&emsp; + Install XAMPP on Microsoft Windows Server 2022 Base instance <br>&emsp; + Deploy application on Windows Server 2022 instance | 25/09/2025 | 26/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Learn about resource usage limitation with IAM service <br> - **Practice:** <br>&emsp; + Allow EC2 usage in Singapore region only <br>&emsp; + Restrict EC2 usage by Instance Family and Instance Type <br>&emsp; + Restrict EBS Volume Storage Type <br>&emsp; + Restrict EC2 deletion by company IP address <br>&emsp; + Restrict EC2 deletion by time window | 26/09/2025 | 26/09/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 3 Achievements:

* Gained deeper understanding of advanced VPN configuration and security, including StrongSwan, Transit Gateway, BGP Dynamic Routing, and monitoring/troubleshooting techniques.  
* Successfully implemented VPN connections across multiple VPCs with flexible and reliable architectures.  
* Strengthened knowledge and hands-on skills in EC2 for both Linux and Windows:  
  * Created, configured, and connected to EC2 instances.  
  * Managed EC2 resources (EBS, Security Group, AMI).  
  * Recovered access using User Data and SSM Session Manager.  
  * Managed custom AMIs, snapshots, and recovery processes.  
* Deployed real-world applications (Node.js and User Management) on Amazon Linux 2 and Windows Server 2022.  
* Applied IAM Policy to restrict resource usage, enhancing security and cost control.  
* Improved cloud operations, deployment, and infrastructure management skills at an advanced practical level.  
