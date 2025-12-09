---
title: "Blog 1"
#date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# AWS Transfer Family SFTP connectors now support VPC-based connectivity

Many organizations rely on the [Secure File Transfer Protocol (SFTP)](https://aws.amazon.com/what-is/sftp/) as the industry standard for exchanging critical business data. Traditionally, securely connecting to private SFTP servers required custom infrastructure, manual scripting, or exposing endpoints to the public internet.

Today, [AWS Transfer Family](https://aws.amazon.com/aws-transfer-family/) [SFTP connectors](https://docs.aws.amazon.com/transfer/latest/userguide/creating-connectors.html) now support connectivity to remote SFTP servers through [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/vpc/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) environments. You can transfer files between [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) and private or public SFTP servers while applying the security controls and network configurations already defined in your VPC. This capability helps you integrate data sources across on-premises environments, partner-hosted private servers, or internet-facing endpoints, with the operational simplicity of a fully managed [Amazon Web Services (AWS)](https://aws.amazon.com/) service.

---

## New capabilities with SFTP connectors

The following are the key enhancements:

- **Connect to private SFTP servers** – SFTP connectors can now reach endpoints that are only accessible within your AWS VPC connection. These include servers hosted in your VPC or a shared VPC, on-premises systems connected over [AWS Direct Connect](https://aws.amazon.com/directconnect/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), and partner-hosted servers connected through VPN tunnels.
- **Security and compliance** – All file transfers are routed through the security controls already applied in your VPC, such as [AWS Network Firewall](https://aws.amazon.com/network-firewall/?nc2=h_prod_se_netf&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) or centralized ingress and egress inspection. Private SFTP servers remain private and don’t need to be exposed to the internet. You can also present static Elastic IP or bring your own IP (BYOIP) addresses to meet partner allowlist requirements.
- **Performance and simplicity** – By using your own network resources such as NAT gateways, AWS Direct Connect or VPN connections, connectors can take advantage of higher bandwidth capacity for large-scale transfers. You can configure connectors in minutes through the [AWS Management Console](https://console.aws.amazon.com/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el),  [AWS Command Line Interface (AWS CLI)](https://aws.amazon.com/cli/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), or [AWS SDKs](https://aws.amazon.com/tools/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) without building custom scripts or third-party tools.

---

## How VPC- based SFTP connections work

SFTP connectors use [Amazon VPC Lattice](https://aws.amazon.com/vpc/lattice/) resources to establish secure connectivity through your VPC. Key constructs include a **resource configuration** and a **resource gateway**. The resource configuration represents the target SFTP server, which you specify using a private IP address or public DNS name. The resource gateway provides SFTP connector access to these configurations, enabling file transfers to flow through your VPC and its security controls.

The following architecture diagram illustrates how traffic flows between Amazon S3 and remote SFTP servers.

![Diagram showing the data flow between Amazon S3 and remote SFTP servers](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/10/14/Screenshot-2025-10-14-at-6.01.04%E2%80%AFPM.png)

As shown in the architecture, traffic flows from Amazon S3 through the SFTP connector into your VPC. A resource gateway is the entry point that handles inbound connections from the connector to your VPC resources. Outbound traffic is routed through your configured egress path, using Amazon VPC NAT gateways with Elastic IPs for public servers or AWS Direct Connect and VPN connections for private servers. You can use existing IP addresses from your VPC CIDR range, simplifying partner server allowlists. Centralized firewalls in the VPC enforce security policies, and customer-owned NAT gateways provide higher bandwidth for large-scale transfers.

---

## When to use this feature

With this capability, developers and IT administrators can simplify workflows while meeting security and compliance requirements across a range of scenarios:

- **Hybrid environments** – Transfer files between Amazon S3 and on-premises SFTP servers using AWS Direct Connect or [AWS Site-to-Site VPN](https://aws.amazon.com/vpn/site-to-site-vpn/), without exposing endpoints to the internet.
- **Partner integrations** – Connect with business partners’ SFTP servers that are only accessible through private VPN tunnels or shared VPCs. This avoids building custom scripts or managing third-party tools, reducing operational complexity.
- **Regulated industries** – Route file transfers through centralized firewalls and inspection points in VPCs to comply with financial services, government, or healthcare security requirements.
- **High-throughput transfers** – Use your own network configurations such as NAT gateways, AWS Direct Connect, or VPN connections with Elastic IP or BYOIP to handle large-scale, high-bandwidth transfers while retaining IP addresses already on partner allowlists.
- **Unified file transfer solution** – Standardize on Transfer Family for both internal and external SFTP connectivity, reducing fragmentation across file transfer tools.

---

## Start building with SFTP connectors

To begin transferring files with SFTP connectors through my VPC environment, I follow these steps:

First, I configure my VPC Lattice resources. In the [Amazon VPC console](https://us-east-1.console.aws.amazon.com/vpcconsole/home/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el), under **PrivateLink and Lattice** in the navigation pane, I choose **Resource gateways**, choose **Create resource gateway** to create one to act as the ingress point into my VPC.

![Image of step 1](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/Create-or-select-a-resource-gateway-1.png)

Next, under **PrivateLink and Lattice** in the navigation pane, I choose **Resource configuration** and choose **Create resource configuration** to create a resource configuration for my target SFTP server. Specify the private IP address or public DNS name, and the port (typically 22).

![Image of step 2](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/create-resouce-configurations.png)

Then, I configure [AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/?nc2=type_a&?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) permissions. I ensure that the IAM role used for connector creation has <span style="color: red; background-color: #f0f0f0">transfer:*</span> permissions, and VPC Lattice permissions <span style="color: red; background-color: #f0f0f0">(vpc-lattice:CreateServiceNetworkResourceAssociation, vpc-lattice:GetResourceConfiguration, vpc-lattice:AssociateViaAWSService</span>). I update the trust policy on the IAM role to specify <span style="color: red; background-color: #f0f0f0">transfer.amazonaws.com</span> as a trusted principal. This enables AWS Transfer Family to assume the role when creating and managing my SFTP connectors.

After that, I create an SFTP connector through the [AWS Transfer Family console](https://console.aws.amazon.com/transfer/home?refid=30641bb5-5f59-4f87-9a27-a89f5ad26ab6). I choose **SFTP Connectors** and then choose **Create SFTP connector.**

![Image of step 3](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/create-SFTP-connector-1.png)

In the **Connector configuration** section, I select **VPC Lattice** as the egress type, then provide the Amazon Resource Name (ARN) of the **Resource Configuration**, **Access role**, and **Connector credentials**. Optionally, include a trusted host key for enhanced security, or override the default port if my SFTP server uses a nonstandard port.

![Image of step 4](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/09/30/configure-SFTP-connector-1.png)

Next, I test the connection. On the **Actions** menu, I choose **Test connection** to confirm that the connector can reach the target SFTP server.

![Image of step 5](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/10/03/test-SFTP-connector-2.png)

Finally, after the connector status is **ACTIVE**, I can begin file operations with my remote SFTP server programmatically by calling Transfer Family APIs such as <span style="color: red; background-color: #f0f0f0">StartDirectoryListing</span>, <span style="color: red; background-color: #f0f0f0">StartFileTransfer</span>, <span style="color: red; background-color: #f0f0f0">StartRemoteDelete</span>, or <span style="color: red; background-color: #f0f0f0">StartRemoteMove</span>. All traffic is routed through my VPC using my configured resources such as NAT gateways, AWS Direct Connect, or VPN connections together with my IP addresses and security controls.

For the complete set of options and advanced workflows, refer to the [AWS Transfer Family documentation](https://docs.aws.amazon.com/transfer/).

---

## Now available

SFTP connectors with VPC-based connectivity are now available in 21 [AWS Regions](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region). Check the [AWS Services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/?utm_source=chatgpt.com/?trk=ba8b32c9-8088-419f-9258-82e9375ad130&sc_channel=el) for the latest supported AWS Regions. You can now securely connect AWS Transfer Family SFTP connectors to private, on-premises, or internet-facing servers using your own VPC resources such as NAT gateways, Elastic IPs, and network firewalls.

— [Betty](https://www.linkedin.com/in/zhengyubin714/)