---
title: "Week 12 Blog: Capstone Project Optimization & Preparing the Cloud Engineer Career Journey"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12. </b> "
---
## Week 12 Objectives
The final week focuses on deep refinement, comprehensive end-to-end testing of the entire Capstone Project, and preparing the technical foundation and professional profiles to successfully enter the job market as a Cloud Engineer:

1. **Architecture & Cost Optimization:** Rigorously apply the AWS Well-Architected Framework to review the entire system, eliminating redundant resources to optimize costs and enhance operational performance.
2. **Security & DR Validation:** Simulate incident scenarios to test Disaster Recovery (DR) capabilities, configure automated backups, and complete the Production Readiness Checklist.
3. **Technical Documentation & Portfolio Preparation:** Package all source code and architectural diagrams, author detailed operational runbooks on a personal Portfolio Blog, and mentally prepare for upcoming technical job interviews.

---

## Week 12 Roadmap Journal

| Day | Task | Start Date | End Date | Documentation / Reference |
| :--- | :--- | :--- | :--- | :--- |
| **07/07** | End-to-End Architecture Review: Overall evaluation of links and integrations between services in the system | 07/07/2026 | 07/07/2026 | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) |
| **08/07** | Performance & Cost Optimization: Review resource sizes (Right-sizing) to optimize expenditures | 08/07/2026 | 08/07/2026 | [AWS Cost Optimization Pillars](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| **09/07** | Security & DR Validation: Verify firewalls, access permissions, and drill disaster recovery scenarios | 09/07/2026 | 09/07/2026 | [AWS Reliability Pillar: Disaster Recovery](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/disaster-recovery-dr.html) |
| **10/07** | Production Readiness Checklist: Review security and operational standards before final handoff | 10/07/2026 | 10/07/2026 | [AWS Operational Excellence Pillar](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/welcome.html) |
| **11/07** | Technical Documentation & Runbook: Draft engineer-standard operational and troubleshooting guides | 11/07/2026 | 11/07/2026 | [AWS Systems Manager Runbooks](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) |
| **12/07** | Final Demo & Presentation: Finalize infrastructure testing scripts and visualize architectural diagrams | 12/07/2026 | 12/07/2026 | [AWS Architecture Icons & Diagrams](https://aws.amazon.com/architecture/icons/) |
| **13/07** | Portfolio & Career Preparation: Sync source code to GitHub and clean up the blog UI for recruiter engagement | 13/07/2026 | 13/07/2026 | Hugo Portfolio Blog |

### Practical Evidence:

# 1. Production Hardening & Capstone Delivery

**System Architecture Model**

![](/images/mohinh.png)

---

## 2. Lab Objectives
*   **High Availability & Fault Tolerance:** Design a system featuring auto-scaling and Multi-AZ fault tolerance.
*   **Production Security Hardening:** Apply the Principle of Least Privilege, completely isolating Web Servers within Private Subnets and exposing ports strictly through the Application Load Balancer.
*   **System Observability:** Configure CPU performance monitoring via CloudWatch Alarms and set up automated alert notifications through Amazon SNS Email.
*   **Chaos Testing:** Simulate a server failure (terminating a virtual machine instance) to practically validate the infrastructure's self-healing capabilities with zero downtime.

---

## 3. Components and Purpose of Use

| AWS Component | Purpose of Use within the System |
| :--- | :--- |
| **Amazon VPC** | Creates an isolated virtual network environment (`10.0.0.0/16`) split across 2 Public Subnets and 2 Private Subnets. |
| **NAT Gateway** | Deployed in the Public Subnet to allow EC2 instances in the Private Subnet to access the Internet to download Nginx and source code, while blocking inbound connections from the Internet. |
| **Launch Template** | A blueprint configuration containing virtual machine specifications (`t3.micro`), OS (Amazon Linux 2023), Security Groups, and a bootstrap script to deploy a dynamic Portfolio UI using IMDSv2. |
| **Application Load Balancer (ALB)** | An Internet-facing gateway that captures public traffic, intelligently routes Port 80, and balances the data load across the backend EC2 fleet. |
| **Auto Scaling Group (ASG)** | Manages the EC2 fleet lifecycle (Min: 2, Desired: 2, Max: 4), automatically scaling out when CPU usage exceeds 70%, or replacing unhealthy instances automatically upon failure detection. |
| **CloudWatch + SNS** | Closely monitors the `CPUUtilization` metric and automatically triggers alert emails whenever the system breaches the defined safety thresholds. |

---

## 4. Detailed Implementation Guide

### Step 1: Initialize the Foundational Network (VPC Networking)
Create a VPC named `capstone-production-vpc` with the CIDR block `10.0.0.0/16`. Configure it to automatically span 2 Availability Zones (`ap-southeast-1a` and `ap-southeast-1b`), establishing 2 public subnets for inbound traffic and 2 private subnets to securely isolate the Web Servers. Deploy 1 NAT Gateway within the public zone to facilitate package downloads for internal resources.

### Step 2: Set Up the Application Load Balancer (ALB)
Create a Target Group named `tg-capstone-production` using the Instance target type on Port 80. Proceed to provision an Internet-facing Application Load Balancer named `alb-capstone-prod`. Configure the Network Mapping to point precisely to the **Public Subnets** across both AZs to guarantee external traffic can enter through the Load Balancer gateway.

### Step 3: Create a Launch Template with Premium Portfolio Source Code
Modify or create a new Launch Template named `capstone-web-template`, utilizing the optimized `t3.micro` instance type. Under Advanced Details, embed a User Data script that automatically installs Nginx and configures a dynamic Glassmorphic Portfolio UI. The script securely requests metadata using **IMDSv2 Tokens** to dynamically fetch instance details:

    #!/bin/bash
    dnf update -y
    dnf install -y nginx
    systemctl start nginx
    systemctl enable nginx

    # Lấy Metadata bảo mật qua IMDSv2 Token
    TOKEN=$(curl -s -X PUT "[http://169.254.169.254/latest/api/token](http://169.254.169.254/latest/api/token)" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
    INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" [http://169.254.169.254/latest/meta-data/instance-id](http://169.254.169.254/latest/meta-data/instance-id))
    AZ=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" [http://169.254.169.254/latest/meta-data/placement/availability-zone](http://169.254.169.254/latest/meta-data/placement/availability-zone))

    # Ghi đè file html Portfolio giao diện Glassmorphism
    cat <<EOF> /usr/share/nginx/html/index.html
    ... (Đoạn mã code giao diện HTML/CSS) ...
    EOF
    systemctl restart nginx

### Step 4: Deploy the Auto Scaling Group (ASG)
Initialize an Auto Scaling Group named `asg-capstone-prod` pointing directly to the latest version (Version Latest) of your Launch Template. Distribute the EC2 fleet across the 2 Private Subnets to completely shield the web servers from the public Internet. Configure the capacity settings with Desired: 2, Min: 2, and Max: 4. Enable a Target Tracking Scaling Policy based on a `CPUUtilization` threshold of 70%.

![](/images/Auto-Scaling.png)
*Evidence: The Auto Scaling Group stably maintaining a healthy 2/2 instance state.*

### Step 5: Enforce Infrastructure Security (Production Hardening)
Conduct a security audit. Inside the Web Server Security Group (`sg-web-server`), remove the open `0.0.0.0/0` rule on the HTTP port (Port 80). Instead, configure the inbound source rule to point directly to the Security Group ID of the ALB. This process ensures that instances within the Private Subnets strictly accept traffic audited by and passing through the Load Balancer, totally preventing direct attack vectors against the backend servers.

### Step 6: Verify Dynamic Data Output
Once infrastructure synchronization and the Instance Refresh process are complete, navigate to the public DNS Name of the Load Balancer. The browser should display a premium personal Portfolio UI with a crisp Glassmorphism effect and a properly aligned rounded square avatar block according to standard specifications.

![](/images/web2.png)

![](/images/web1.png)

*Evidence: Live website operating via the Load Balancer DNS link, accurately displaying the unique Instance ID and Deployed AZ partition pulled directly from the AWS metadata system.*

### Step 7: Chaos Testing — Destructive Validation and Auto-Recovery
Simulate a live failure scenario by accessing the EC2 Console and executing a Terminate command on one of the active, running instances.

*   **Load Balancing Results:** While continuously hitting F5 on the browser, service availability remains uninterrupted (**Zero Downtime**). The Load Balancer immediately detects the unhealthy instance via its Health Check mechanism and reroutes users to the surviving instance in the opposing AZ. The web UI smoothly updates the displayed instance metadata to reflect the active node.
*   **Self-Healing Results:** After waiting approximately 2 minutes, the Auto Scaling Group recognizes that the running capacity has dropped below the desired threshold. It instantly triggers an automated launch sequence to provision a brand-new EC2 instance, successfully restoring the infrastructure to its balanced, healthy state.

![](/images/report.png)

![](/images/email.png)

---

## Week 12 Key Takeaways

*   **Deep Understanding of AWS Networking Traffic Flows:** Gained clear clarity on the distinct roles of Security Groups across various layers (the ALB must expose its ports openly to the Internet `0.0.0.0/0`, whereas EC2 instances must explicitly trust traffic coming only from that specific ALB).
*   **The Criticality of IMDSv2:** Mastered the updated token-based security architecture implemented in Amazon Linux 2023 when using curl commands to fetch system metadata, preventing blank interface elements during execution.
*   **Enterprise Architecture Design Mindset:** Moving beyond simply building working environments, a true Cloud Engineer must understand how to safely isolate compute nodes within Private Subnets and configure auto-scaling and proactive monitoring patterns to optimize operational expenses while guaranteeing 24/7 high availability under pressure.
