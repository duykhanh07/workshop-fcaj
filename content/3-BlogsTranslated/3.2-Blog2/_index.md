---
title: "Blog 2"
#date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS Weekly Roundup: Amazon Quick Suite, Amazon EC2, Amazon EKS, and more (October 13, 2025)

This week I was at the inaugural [AWS AI in Practice meetup from the AWS User Group UK](https://www.meetup.com/awsuguk-ai-in-practice/). AI-assisted software development and agents were the focus of the evening! Next week I’ll be in Italy for [Codemotion](https://conferences.codemotion.com/milan2025/agenda/) (Milan) and an [AWS User Group meetup](https://www.meetup.com/amazon-web-services-rome/events/311302816) (Rome). My sessions there will be about AI agents and context engineering. I am also excited to [try the new Amazon Quick Suite](https://aws.amazon.com/blogs/aws/reimagine-the-way-you-work-with-ai-agents-in-amazon-quick-suite/) that brings AI-powered research, business intelligence, and automation capabilities into a single workspace.

---

## Last week’s launches

Here are the launches that got my attention this week:

- [Amazon Quick Suite](https://aws.amazon.com/quicksuite/) – A new agentic teammate that quickly answers your questions at work and turns those insights into actions for you. [Read more in Esra’s launch post](https://aws.amazon.com/blogs/aws/reimagine-the-way-you-work-with-ai-agents-in-amazon-quick-suite/).
- [Amazon EC2](https://aws.amazon.com/ec2/) – General-purpose [M8a instances](https://aws.amazon.com/blogs/aws/new-general-purpose-amazon-ec2-m8a-instances-are-now-available/) powered by the 5th Generation AMD EPYC (codename Turin) processors and compute-optimized [C8i and C8i-flex instances](https://aws.amazon.com/blogs/aws/introducing-new-compute-optimized-amazon-ec2-c8i-and-c8i-flex-instances/) powered by custom Intel Xeon 6 processors are now available.
- [Amazon EKS](https://aws.amazon.com/eks/) – EKS and [EKS Distro](https://aws.amazon.com/eks/eks-distro/) [now support Kubernetes version 1.34](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-eks-distro-kubernetes-version-1-34/) with several improvements.
- [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/) – AWS Key Management Service keys can now be used to [encrypt identity data stored in IAM Identity Center organization instances](https://aws.amazon.com/blogs/aws/aws-iam-identity-center-now-supports-customer-managed-kms-keys-for-encryption-at-rest/).
- [Amazon VPC Lattice](https://aws.amazon.com/vpc/lattice/) – You can now [configure the number of IPv4 addresses assigned to resource gateway elastic network interfaces (ENIs)](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-vpc-lattice-configurable-ip-resource-gateway/). The IPv4 addresses are used for network address translation and determine the maximum number of concurrent IPv4 connections to a resource
- [Amazon Q Developer](https://aws.amazon.com/q/developer/) – Amazon Q Developer can help you [get information about AWS product and service pricing, availability, and attributes](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-q-developer-understand-service-prices-estimate-workload-costs/), making it easier to select the right resources and estimate workload costs using natural language. [More info in this blog post](https://aws.amazon.com/blogs/aws-cloud-financial-management/introducing-aws-pricing-capabilities-in-amazon-q-developer-ask-questions-get-instant-cost-insights/).
- [Amazon RDS for Db2](https://aws.amazon.com/rds/db2/) – You can now [perform native database-level backups](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-rds-for-db2-native-database-backup/), offering greater flexibility in database management and migration.
- [AWS Service Quotas](https://docs.aws.amazon.com/servicequotas/latest/userguide/intro.html) – Get notified of your quota usage with [automatic quota management](https://aws.amazon.com/about-aws/whats-new/2025/10/automatic-quota-management-service-quotas/). Configure your preferred notifications channels, such as email, SMS, or Slack. Notifications are also available in [AWS Health](https://docs.aws.amazon.com/health/latest/ug/what-is-aws-health.html), and you can subscribe to related [AWS Cloudtrail](https://aws.amazon.com/cloudtrail/) events for automation workflows.
- [Amazon Connect](https://aws.amazon.com/connect/) – You can now [programmatically enrich case data with the new case APIs](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-cases-api-link-search/) to link related cases, add custom related items, and search across them. You can now also [customize service level calculations](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-enables-service-level-calculation-configuration/) to your specific needs. New capabilities that have just been introduced include [copy and bulk edit of agent scheduling configuration](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-copy-bulk-edit-agent-scheduling/) and [agent schedule adherence notifications](https://aws.amazon.com/about-aws/whats-new/2025/10/amazon-connect-agent-adherence-notifications/).
- [AWS Client VPN](https://aws.amazon.com/vpn/) – Now [supports MacOS Tahoe](https://aws.amazon.com/about-aws/whats-new/2025/10/aws-client-vpn-macos-tahoe/).

---

## Additional updates

Here are some additional projects, blog posts, and news items that I found interesting:

- [Serverless ICYMI Q3 2025](https://aws.amazon.com/blogs/compute/serverless-icymi-q3-2025/) – A quarterly recap of serverless news, in case you missed it.
- [Best practices for migrating from Apache Airflow 2.x to Apache Airflow 3.x on Amazon MWAA](https://aws.amazon.com/blogs/big-data/best-practices-for-migrating-from-apache-airflow-2-x-to-apache-airflow-3-x-on-amazon-mwaa/) – A guide to help get the benefit of the new release.
- [Building self-managed RAG applications with Amazon EKS and Amazon S3 Vectors](https://aws.amazon.com/blogs/storage/building-self-managed-rag-applications-with-amazon-eks-and-amazon-s3-vectors/) – A reference architecture for building and deploying a self-managed RAG application using open source tools such as [Ray](https://docs.ray.io/en/latest/ray-overview/index.html), [Hugging Face](https://huggingface.co/), and [LangChain](https://www.langchain.com/).
- [BBVA: Building a multi-region, multi-country global Data and ML Platform at scale](https://aws.amazon.com/blogs/industries/part-1-bbva-building-a-multi-region-multi-country-global-data-and-ml-platform-at-scale/) – A six-part series of posts describing the journey to transform [BBVA](https://www.bbva.com/en/) entire data analytics infrastructure with one of the largest and most complex cloud migrations in the banking sector.
- [Customizing text content moderation with Amazon Nova](https://aws.amazon.com/blogs/machine-learning/customizing-text-content-moderation-with-amazon-nova/) – Fine-tune Amazon Nova for content moderation tasks tailored to your requirements using domain-specific training data and organization-specific moderation guidelines.

---

## Upcoming AWS events

Check your calendars so that you can sign up for these upcoming events:

- [AWS AI Agent Global Hackathon](https://info.devpost.com/blog/aws-ai-agent-global-hackathon?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) – This is your chance to dive deep into our powerful generative AI stack and create something truly awesome. From September 8th to October 20th, you have the opportunity to create AI agents using AWS suite of AI services, competing for over $45,000 in prizes and exclusive go-to-market opportunities.
- [AWS Gen AI Lofts](https://aws.amazon.com/startups/lp/aws-gen-ai-lofts?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) – You can learn AWS AI products and services with exclusive sessions, meet industry-leading experts, and have valuable networking opportunities with investors and peers. Register in your nearest city: [Paris](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-paris?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (October 7–21), [London](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-london?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (Oct 13–21), and [Tel Aviv](https://aws.amazon.com/startups/lp/aws-gen-ai-loft-tel-aviv?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el) (November 11–19).
- [AWS Community Days](https://aws.amazon.com/events/community-day/) – Join community-led conferences that feature technical discussions, workshops, and hands-on labs led by expert AWS users and industry leaders from around the world: [Budapest](https://awscommunity.eu/) (October 16).

Join the [AWS Builder Center](https://builder.aws.com/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el) to learn, build, and connect with builders in the AWS community. Browse here [upcoming in-person events](https://aws.amazon.com/events/explore-aws-events/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el), [developer-focused events](https://aws.amazon.com/developer/events/?trk=e61dee65-4ce8-4738-84db-75305c9cd4fe&sc_channel=el), and [events for startups](https://aws.amazon.com/startups/events?trk=c4ea046f-18ad-4d23-a1ac-cdd1267f942c&sc_channel=el).

That’s all for this week. Check back next Monday for another [Weekly Roundup](https://aws.amazon.com/blogs/aws/tag/week-in-review/?trk=7c8639c6-87c6-47d6-9bd0-a5812eecb848&sc_channel=el)!

– [Danilo](https://x.com/danilop)                


