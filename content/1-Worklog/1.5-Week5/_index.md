---
title: "Week 5 Blog: Advanced Cloud Operations & Production Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives:

This week focuses on implementing a **Production-Ready Architecture**, transitioning from experimental models to a highly available, strictly secured, and fault-tolerant system on AWS:
1. **Secure & Isolated Network Design:** Master advanced VPC architecture using a Multi-AZ model, completely isolating the compute environment within Private Subnets and enforcing strict access control via a secure Bastion Host (Jump Box).
2. **Secure Routing & Connection Management:** Efficiently operate a NAT Gateway for outbound traffic from the internal infrastructure without exposing private IP addresses to the public Internet.
3. **High Availability & Fault Tolerance (HA/DR):** Design and deploy an automatically scaling and self-healing Multi-AZ architecture, combined with a comprehensive Backup & Disaster Recovery strategy to maximize data protection against disruption.
4. **Edge Security Hardening:** Deploy web application layer defense shields to protect against DDoS attacks and common vulnerabilities using AWS WAF and AWS Shield.
5. **Hands-On Production Deployment (Mini Production Project):** Consolidate all knowledge into a complete real-world system, visualize the system structure (Architecture Diagram), and document it professionally on a Technical Blog.

## Week 5 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :---: | :--- | :---: | :---: | :--- |
| **19/05** | Advanced VPC & Bastion Architecture: Understand production network models | 19/05/2026 | 19/05/2026 | [AWS VPC Docs](https://docs.aws.amazon.com/vpc/) |
| **20/05** | NAT Gateway, Private Subnet & Secure Access: Build a private system | 20/05/2026 | 20/05/2026 | AWS Networking Lab |
| **21/05** | Backup & Disaster Recovery on AWS: Ensure data safety | 21/05/2026 | 21/05/2026 | [AWS Backup Service](https://aws.amazon.com/backup/) |
| **22/05** | High Availability Multi-AZ Architecture: Design fault-tolerant systems | 22/05/2026 | 22/05/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| **23/05** | WAF, Shield & Security Hardening: Protect web systems | 23/05/2026 | 23/05/2026 | [AWS WAF & Shield](https://aws.amazon.com/waf/) |
| **24/05** | Mini Production Project Deployment: Construct a complete system | 24/05/2026 | 24/05/2026 | Self-deployed |
| **25/05** | Documentation, Diagram & Technical Blog Writing: Synthesize and report | 25/05/2026 | 25/05/2026 | Hugo Portfolio Blog |

---
### Technical Evidence:

**System Architecture Diagram**
![](/images/mohinh.png)

## 1. Lab Objectives

This comprehensive lab implements a resilient, production-ready cloud infrastructure focused on 3 core goals:
* **High Availability (HA):** Design a Multi-AZ architecture distributing resources across independent data centers to eliminate any Single Point of Failure.
* **Defense in Depth:** Completely isolate application servers inside Private Subnets, tightly control administrative traffic through a Bastion Host, and deploy **AWS WAF** at the application layer (Layer 7) to filter malicious traffic.
* **Disaster Recovery (DR):** Automate system snapshot workflows to provide comprehensive protection against unexpected disasters.

---

## 2. Component Directory & Use Cases

| AWS Component | Technical Role & Use Case |
| :--- | :--- |
| **Advanced VPC** | Divides the network into 2 Internet-facing Public Subnets (`10.0.0.0/24`, `10.0.1.0/24`) and 2 completely isolated Private Subnets (`10.0.128.0/24`, `10.0.129.0/24`). |
| **Bastion Host** | Acts as the single secure entry point in the Public zone, used for administrative management (internal SSH) of the application servers in the Private zone. |
| **NAT Gateway** | Allows servers in the Private Subnets to make Outbound connections to the Internet for OS patches while blocking the public Internet from initiating inbound connections. |
| **Application Load Balancer (ALB)** | Receives and distributes user traffic evenly across two AZs to ensure performance and fault tolerance. |
| **AWS WAF (Web ACL)** | A Layer 7 application firewall that protects against common exploits like SQL Injection and Cross-Site Scripting (XSS). |
| **AWS Backup** | Provides centralized backup management, automating scheduled EC2 instances snapshot workflows. |

---

## 3. Step-by-Step Implementation Guide

### Step 1: Initialize the Secure Network (Advanced VPC Architecture)
1. Navigate to the **VPC Dashboard** -> click **Create VPC** -> select the **VPC and more** option.
2. Configure the infrastructure parameters:
   * **Name tag auto-generation**: `Production-VPC`
   * **IPv4 CIDR block**: `10.0.0.0/16`
   * **Number of Availability Zones (AZs)**: `2` (AZ A and AZ B).
   * **Number of Public subnets**: `2`
   * **Number of Private subnets**: `2`
   * **NAT Gateways**: Select **In 1 AZ** to optimize costs for this lab environment.
3. Click **Create VPC** and wait for the system to automatically generate and map the Route Tables.

### Step 2: Configure Security Groups (Instance-Level Firewalls)
Create 3 core security groups:
* **Bastion-SG:** Allow Inbound port `22` (SSH) exclusively from the administrator's local IP address (`My IP`).
* **ALB-SG:** Allow Inbound port `80` (HTTP) from the entire public Internet (`0.0.0.0/0`).
* **WebServer-SG:** Enforce strict infrastructure security:
  * Allow port `80` (HTTP) inbound traffic only when the `Source` is explicitly set to **ALB-SG**.
  * Allow port `22` (SSH) administrative traffic only when the `Source` is explicitly set to **Bastion-SG**.

### Step 3: Deploy EC2 Instances
1. Launch 1 instance named `Bastion-Host` inside `Public Subnet 1`, check the **Enable Auto-assign public IP** option, and attach the `Bastion-SG` group.
2. Launch 2 Web Server instances completely inside the isolated private network zones, disable Public IP assignment, use the same key pair, and apply the `WebServer-SG` group:
   * `Web-Server-A` placed in `Private Subnet 1`.
   * `Web-Server-B` placed in `Private Subnet 2`.
3. Bootstrap the instances using the following initialization script (**User data**) to configure the application environment:
    ```bash
    #!/bin/bash
    sudo dnf update -y
    sudo dnf install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
    echo "<h1>Hello from Web Server (Private AZ)</h1>" > /var/www/html/index.html
    ```

### Step 4: Configure High Availability Load Balancing with ALB
1. Navigate to **Target Groups** -> create a new Instance-based target group named `Web-TG` operating on port `80`, then register both `Web-Server-A` and `Web-Server-B`.
2. Navigate to **Load Balancers** -> create an **Application Load Balancer** named `Prod-ALB` and configure it as **Internet-facing**.
3. Under **Network mapping**, map the ALB to the 2 Public Subnets across both AZs and assign the `ALB-SG` security group.
4. Under **Listeners and routing**, configure the default routing rule to forward all traffic to the `Web-TG` target group.

### Step 5: Enable Application Layer Shielding with AWS WAF
1. Open the **WAF & Shield** service console -> select **Protection packs (web ACLs)** -> click **Create protection pack (web ACL)**.
2. Configure the application details:
   * **App category**: Select `Other` (or `General`).
   * **App focus**: Keep the default `Both API and web`.
3. Under **Select resources to protect**, choose **Add regional resources** -> Select **Application Load Balancer** -> link it directly to your `Prod-ALB`.
4. Under **Choose initial protections**, check the **Recommended rules for you** template pack to automatically activate core rule sets that protect against SQL Injection and filter known malicious IP reputations.
5. Name the protection pack `Prod-Web-ACL` and complete the creation process.

### Step 6: Configure Disaster Recovery (DR) Backups
1. Navigate to **AWS Backup** -> go to **Backup vaults** -> click **Create Backup vault** to create an encrypted storage destination named `Prod-Backup-Vault`.
2. Switch to **Backup plans** -> click **Create backup plan** -> choose **Build a new plan** from scratch and name it `Prod-Daily-Backup`.
3. Set up the *Backup rule* parameters: Frequency set to **Daily**, retention period set to **30 days** (change the unit from *Years* to *Days* to avoid unnecessary costs), and designate `Prod-Backup-Vault` as the destination.
4. From the newly created backup plan interface, click **Assign resources** -> choose **Include specific resource types** -> select **EC2** and explicitly target the instance IDs of `Web-Server-A` and `Web-Server-B`.

### Step 7. Verification, Testing & Real-World Lessons

#### Verification 1: Multi-AZ Load Balancing and Failover Capabilites
1. Access the ALB using its DNS Name URL via an independent incognito browser window. Refreshing the page continuously (**F5**) demonstrates that traffic is distributed smoothly and alternately between the two data centers:
   * **Request 1:** Displays `Hello from Web Server A (Private AZ-A)`.
   * **Request 2:** Displays `Hello from Web Server B (Private AZ-B)`.
2. **Failover Experiment:** When manually stopping (`Stop`) `Web-Server-A`, the Application Load Balancer quickly detects the unhealthy status via target Health Checks and instantly shifts all user connections to `Web-Server-B`, maintaining zero service downtime.

![](/images/WebA.png)
![](/images/WebB.png)

#### Verification 2: Simulated Exploit Testing against AWS WAF
1. Simulating an attacker attempting a Layer 7 malicious web request by executing a SQL Injection payload directly via the URL parameter:
    ```text
    http://<DNS_NAME_ALB>/?id=1'+OR+1=1+UNION+SELECT+null,username,password+FROM+users--
    ```

![](/images/403WAF.png)
