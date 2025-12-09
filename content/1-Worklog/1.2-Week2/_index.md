---
title: "Worklog Week 2"
#date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Understand and practice AWS core infrastructure services, including VPC, EC2, and Site-to-Site VPN.  
* Learn how to design, configure, and secure a basic VPC network.  
* Deploy and manage EC2 Instances within Public and Private Subnets.  
* Grasp the concept of network connectivity between on-premises and AWS Cloud through Site-to-Site VPN.

### Tasks to be implemented this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Start Date  | Completion Date | Resources                                                              |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------------- | ---------------------------------------------------------------------- |
| 2   | - Learn the basics of VPC: <br>&emsp; + Subnets <br>&emsp; + Route Table <br>&emsp; + Internet Gateway <br>&emsp; + NAT Gateway <br>&emsp; + Security Groups <br>&emsp; + Network Access Control Lists <br> - **Practice:** Build a VPC environment by deploying essential AWS networking components (VPC, Subnets, Route Table, Internet Gateway, Security Groups, VPC Flow Logs) and design a secure, scalable network structure. | 15/09/2025   | 15/09/2025       | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and AWS Study Group YouTube |
| 3   | - Learn the fundamentals of EC2: <br>&emsp; + Concept, features, and operation of EC2 <br>&emsp; + Instance types (General Purpose, Compute Optimized, Memory Optimized, etc.) <br>&emsp; + Concepts of AMI, EBS, Security Group, and Key Pair <br>&emsp; + Network, storage, and access configuration for EC2 Instances <br>&emsp; + Instance lifecycle management (Start, Stop, Reboot, Terminate) | 16/09/2025   | 16/09/2025       | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 4   | - Learn about VPC Reachability Analyzer, EC2 Instance Connect Endpoint, AWS Systems Manager Session Manager, and CloudWatch Monitoring <br> - **Practice:** Deploy Amazon EC2 Instances <br>&emsp; + Create EC2 instances in Public and Private Subnets <br>&emsp; + Connect via SSH <br>&emsp; + Create Elastic IP <br>&emsp; + Create NAT Gateway <br>&emsp; + Configure Route Table for Private Subnet <br>&emsp; + Analyze connections using VPC Reachability Analyzer <br>&emsp; + Deploy EIC Endpoint | 17/09/2025   | 18/09/2025       | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 5   | - Learn about AWS Site-to-Site VPN: <br>&emsp; + Virtual Private Gateway (VPG) <br>&emsp; + Customer Gateway (CGW)                                                                                                                            | 18/09/2025   | 18/09/2025       | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6   | - **Practice:** <br>&emsp; + Configure VPC for Site-to-Site VPN <br>&emsp; + Deploy EC2 Instance for Customer Gateway <br>&emsp; + Create Virtual Private Gateway <br>&emsp; + Create Customer Gateway <br>&emsp; + Establish VPN connection between VPC and on-premises environment <br>&emsp; + Configure Customer Gateway <br>&emsp; + Adjust AWS VPN Tunnel settings | 19/09/2025   | 19/09/2025       | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 2 Achievements:

* Gained a solid understanding of VPC architecture and core components, including Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group, and Network ACL.  
* Built a basic, scalable VPC model with secure access between subnets.  
* Mastered fundamental knowledge of Amazon EC2, including:
  * Instance classification and AMI selection  
  * Configuration of storage (EBS), security (Security Group, Key Pair), and networking (Elastic IP, Route Table)  
  * Instance lifecycle management (launch, connect, stop, terminate)  
* Practiced deploying EC2 Instances in Public and Private Subnets, connecting via SSH, and managing access through Instance Connect Endpoint.  
* Learned how to use VPC Reachability Analyzer and CloudWatch for monitoring and analyzing network connections.  
* Understood the concepts, components, and setup process of Site-to-Site VPN between on-premises and AWS Cloud.  
* Successfully practiced creating a secure VPN connection between Customer Gateway and Virtual Private Gateway, extending the on-premises network to AWS Cloud.
