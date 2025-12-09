---
title: "Worklog Week 7"
#date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives:
* Understand and practice AWS Systems Manager – Session Manager, including public/private EC2 connection, setting up Endpoints, and managing session logs.
* Grasp concepts and basic & advanced practices of AWS CloudFormation, including creating templates, creating stacks, StackSets, and Drift Detection.
* Know how to use AWS Cloud9 to work with CloudFormation and the AWS environment.
* Understand and set up AWS IAM Identity Center (AWS SSO) in AWS Organization: Users, Groups, Permission Sets, and access delegation.
* Apply Time-based Access Control and Customer Managed Policies to set up advanced Permission Sets for each user group in the Organization.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --------- | ------------- | ---------------- | -------------- |
| 2 | - Learn about Session Manager <br> - Practice: work with Amazon System Manager - Session Manager <br> &emsp; + Create VPC, Public subnet, Private subnet, security group, public Linux and private Windows servers <br> &emsp; + Create IAM Role <br> &emsp; + Create connection to Public EC2 server <br> &emsp; + Create connection to Private EC2 server <br> &emsp;&emsp; + Enable DNS hostnames <br> &emsp;&emsp; + Create ssm Endpoint <br> &emsp;&emsp; + Create ssmmessages Endpoint <br> &emsp;&emsp; + Create ec2messages Endpoint <br> &emsp;&emsp; + Assign IAM role and restart EC2 instance <br> &emsp; + Manage session logs <br> &emsp;&emsp; + Update IAM Role <br> &emsp;&emsp; + Create S3 bucket and S3 Gateway endpoint <br> &emsp;&emsp; + Monitor session logs <br> &emsp;&emsp; + Check Session logs in S3 <br> &emsp; + Port Forwarding <br> &emsp;&emsp; + Create IAM User with SSM connection permissions <br> &emsp;&emsp; + Install and configure AWS CLI and Session Manager Plugin <br> &emsp;&emsp; + Perform Portforwarding | 20/10/2025 | 20/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 3 | - Learn about AWS CloudFormation and AWS Cloud9 <br> - Practice: create a CloudFormation template and a few basic features of CloudFormation, and how to validate templates <br> &emsp; + Create IAM User, IAM Role <br> &emsp; + Create Workspace <br> &emsp; + Create CloudFormation Template | 21/10/2025 | 21/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/), https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html, https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-template-resource-type-ref.html |
| 4 | - Learn about StackSets, Drift Detection, Simple Queue Service <br> - Practice: Advanced CloudFormation <br> &emsp; + Create Lambda Function <br> &emsp; + Create Stack in CloudFormation <br> &emsp; + Connect EC2 Instance <br> &emsp; + Mappings and Stacksets <br> &emsp; + Change resource configuration with Drift | 22/10/2025 | 22/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) and https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html |
| 5 | - Practice: Set up Single Sign-On (Amazon SSO) for Organization <br> &emsp; + Enable AWS Organizations <br> &emsp; + Enable IAM Identity Center <br> &emsp; + Create Users and Groups in IAM Identity Center <br> &emsp; + Create Permission Sets <br> &emsp; + Provision Permission Sets <br> &emsp; + Check access based on user, group, and permission set <br> &emsp;&emsp; + Deploy CloudFormation Template <br> &emsp;&emsp; + Verify access permissions <br> &emsp;&emsp; + Check Administrator permissions <br> &emsp;&emsp; + Check ReadOnly permissions <br> &emsp; + Configure AWS CLI - Manually refresh credentials <br> &emsp; + Configure AWS CLI - Automatically refresh credentials | 23/10/2025 | 23/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |
| 6 | - Practice: Set up Single Sign-On (Amazon SSO) for Organization <br> &emsp; + Use Time-based access control to provide temporary access to AWS accounts for Security Auditor <br> &emsp;&emsp; + Create Permission Set with necessary access permissions for Security Auditor as inline policy with time-based access control conditions <br> &emsp;&emsp; + Create Group for Security Auditors <br> &emsp;&emsp; + Assign users to Security Auditors Group <br> &emsp;&emsp; + Assign access to AWS Account for Security Auditors group with newly created permission set <br> &emsp; + Create CMP, create permission set by attaching created CMP, and finally provision permission set for AWS account <br> &emsp;&emsp; + Create Customer Managed IAM Policy <br> &emsp;&emsp; + Create permission Set with Customer Managed policy <br> &emsp;&emsp; + Create Group and create user and add to Group <br> &emsp;&emsp; + Assign Permission set to AWS Account and verify access | 24/10/2025 | 24/10/2025 | [cloudjourney.awsstudygroup.com](https://cloudjourney.awsstudygroup.com/) |

### Week 7 Achievements:
* Master Session Manager in AWS Systems Manager: create public/private VPC environment, assign IAM Role, set up SSM, EC2Messages, SSMMessages endpoints, and successfully connect to both Public and Private EC2.
* Manage and track Session Logs: configure S3 bucket, S3 Gateway Endpoint, update IAM Role to log, and verify logs functioning correctly in S3.
* Perform Port Forwarding via SSM: create IAM User, configure AWS CLI + SSM Plugin, perform secure port forwarding without opening ports in Security Group.
* Learn and use AWS CloudFormation: create Cloud9 Workspace, create IAM User/Role, write CloudFormation template, and validate template validity.
* Deploy advanced CloudFormation: create Lambda Function, build Stack, set up StackSets, map resources, connect EC2, and check Drift Detection when changing configuration.
* Set up Single Sign-On with IAM Identity Center for the entire AWS Organization: create Users/Groups, Permission Sets, grant Administrator/ReadOnly permissions, and verify permissions by user/group.
* Configure AWS CLI with IAM Identity Center: test manual and automatic credential refresh, ensuring correct permission set recognition.
* Apply Time-based Access Control to grant temporary access to Security Auditor: create Permission Set with time-based conditions, categorize Security Auditor group, assign permissions, and verify correct operation.
* Create Customer Managed Policy (CMP) and combine CMP into Permission Set, assign to AWS Account, and check actual user access permissions.