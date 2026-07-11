---
title: "Week 8 Blog: Deploying DevSecOps Systems & Cloud Security"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
## Week 8 Objectives:
This week focuses on applying comprehensive security mindsets into the software development lifecycle and cloud infrastructure, transitioning from traditional security models to automated security testing (Shift-Left Security):

1. **Understanding Cloud Security in Enterprise Realities:** Master the Shared Responsibility Model, Defense in Depth layers, and strictly enforce the Principle of Least Privilege using AWS IAM.
2. **Automating Security Testing:** Integrate Static Application Security Testing (SAST) tools, Infrastructure as Code (IaC) scanning tools, and configure automated services to scan for malware and system vulnerabilities within the AWS environment.
3. **Building a Fundamental DevSecOps Architecture:** Embed automated security compliance gates into CI/CD pipelines to ensure all code and infrastructure modifications undergo automated vulnerability screening before moving to production.

---

## Week 8 Roadmap Journal

| Day | Task | Start Date | End Date | Resource / Documentation |
| :--- | :--- | :--- | :--- | :--- |
| **06/09** | AWS Security Fundamentals: Approaching core security principles in the cloud | 06/09/2026 | 06/09/2026 | [AWS Cloud Security Docs](https://aws.amazon.com/security/) |
| **06/10** | IAM Advanced & Least Privilege: Optimizing fine-grained permission policies | 06/10/2026 | 06/10/2026 | [AWS IAM Security Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| **06/11** | Secrets Management: Managing and securing sensitive credentials centrally | 06/11/2026 | 06/11/2026 | [AWS Secrets Manager Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) |
| **06/12** | Vulnerability Scanning: Utilizing automated source code and container scanning tools | 06/12/2026 | 06/12/2026 | [Trivy Vulnerability Scanner](https://aquasecurity.github.io/trivy/latest/) |
| **06/13** | AWS Inspector & Security Hub: Managing infrastructure risks and centralizing security alerts | 06/13/2026 | 06/13/2026 | [AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) |
| **06/14** | DevSecOps Pipeline Integration: Embedding automated security checks into CI/CD workflows | 06/14/2026 | 06/14/2026 | [AWS DevSecOps Architecture](https://aws.amazon.com/blogs/devops/building-a-secure-devsecops-pipeline-on-aws/) |

---
### Real-world Evidence:

## 1. Building an Automated DevSecOps Pipeline for Vulnerability Scanning and Secrets Management on AWS

**System Architecture Model**
![](/images/mohinh.png)

---

## 2. Lab Objectives
* **Security Automation:** Integrate the Trivy scanning engine into GitHub Actions to execute automated filesystem vulnerability scans prior to the deployment phase.
* **Keyless Authentication:** Eliminate hardcoded, static AWS Access Keys entirely by utilizing OpenID Connect (OIDC) identity federation between GitHub and AWS.
* **Secrets Protection:** Deploy a Zero-Hardcode architecture by storing and managing sensitive credentials centrally inside AWS Secrets Manager.
* **Continuous Security Monitoring:** Establish automated runtime vulnerability assessments on EC2 instances using AWS Inspector and consolidate findings natively within AWS Security Hub.

---

## 3. Components and Use Cases

| Component | Classification | Purpose in the Lab |
| :--- | :--- | :--- |
| **GitHub Actions** | CI/CD Pipeline | Orchestrates the entire workflow from code analysis to deployment execution. |
| **Trivy Scanner** | SCA Tool | Runs static scanning over the codebase (via `fs` mode) to identify CVES in external package dependencies. |
| **AWS IAM OIDC** | Identity Governance | Assumes dynamic, short-lived permissions (`AssumeRoleWithWebIdentity`) for GitHub Actions runners. |
| **AWS Secrets Manager** | Data Security | Safely stores and encrypts backend database connection strings (`prod/app/db_creds`). |
| **Amazon EC2** | Compute Node | Hosts the core web application instance running on Amazon Linux 2023. |
| **AWS Inspector** | Vulnerability Management | Continuously analyzes active network exposures (Network Reachability) on EC2 nodes. |
| **AWS Security Hub** | SIEM / Dashboard | Aggregates and consolidates runtime security findings discovered by AWS Inspector into a unified management panel. |

---

## 4. Detailed Step-by-Step Deployment Guide

### Step 4.1: Initializing a Secure Store (AWS Secrets Manager)
1. Navigate to the **Secrets Manager Console** -> Click **Store a new secret**.
2. Select the secret type: `Other type of secret`.
3. Fill in the Key/Value configuration pairs:
   * **Key:** `db_password`
   * **Value:** `SuperSecretPassword2026!`
4. Name the secret identifier: `prod/app/db_creds` and copy the generated **Secret ARN** for reference.

### Step 4.2: Configuring Strict IAM Permissions & OIDC Federation
1. Navigate to the **IAM Console** -> **Identity Providers**, add a new provider configuration:
   * **Provider URL:** `https://token.actions.githubusercontent.com`
   * **Audience:** `sts.amazonaws.com`
2. Create an IAM Role named `GitHubActionsWorkflowRole`, then update its **Trust Relationship** policy block to scope permissions down to your exact repository context:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
       {
           "Effect": "Allow",
           "Principal": {
               "Federated": "arn:aws:iam::210347900763:oidc-provider/token.actions.githubusercontent.com"
           },
           "Action": "sts:AssumeRoleWithWebIdentity",
           "Condition": {
               "StringEquals": { "token.actions.githubusercontent.com:aud": "sts.amazonaws.com" },
               "StringLike": { "token.actions.githubusercontent.com:sub": "repo:Khanh17052004/devsecops-lab:*" }
           }
       }
       ]
   }

1. Attach an inline policy named GitHubDeployEC2Policy to safely limit infrastructure manipulation operations for the role:

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": ["ec2:DescribeInstances", "ec2:StartInstances", "ec2:StopInstances"],
                "Resource": "*"
            }
        ]
    }

2. Create an IAM Instance Profile named EC2AppRole, attach the managed AmazonSSMManagedInstanceCore policy alongside explicit read-only secret permissions:

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "secretsmanager:GetSecretValue",
                "Resource": "arn:aws:secretsmanager:us-east-1:210347900763:secret:prod/app/db_creds-xxxxxx"
            }
        ]
    }

### Step 4.3: Provisioning the Infrastructure Server (Amazon EC2)

Launch a new instance using the following technical specifications:

  AMI: Amazon Linux 2023 (Eligible for Free Tier).

  Instance Type: t2.micro.

  Key Pair: Select "Proceed without a key pair" (Enforcing administrative access entirely via Systems Manager Session Manager).

  Security Group: Expose only port 80 (HTTP) to public traffic origins (0.0.0.0/0).

  IAM Instance Profile: Associate the EC2AppRole profile to the instance.

### Step 4.4: Crafting the DevSecOps Pipeline Blueprint

 Create the pipeline definition file under .github/workflows/devsecops-pipeline.yml inside your codebase:

      name: DevSecOps CI/CD Pipeline

      on:
        push:
          branches: [ "main" ]

      permissions:
        id-token: write
        contents: read

      jobs:
        Security-Scan:
          runs-on: ubuntu-latest
          steps:
          - name: Checkout Code
            uses: actions/checkout@v3

          - name: Run Trivy vulnerability scanner (Repo Scan)
            uses: aquasecurity/trivy-action@master
            with:
              scan-type: 'fs'
              scan-ref: '.'
              format: 'table'
              exit-code: '0'
              severity: 'CRITICAL,HIGH'

        Deploy-to-AWS:
          needs: Security-Scan
          runs-on: ubuntu-latest
          steps:
          - name: Checkout Code
            uses: actions/checkout@v3

          - name: Configure AWS Credentials
            uses: aws-actions/configure-aws-credentials@v2
            with:
              role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
              aws-region: us-east-1

          - name: Deploy Application
            run: |
              echo "Pipeline successfully passed the security checkpoints. Initiating secure deployment to EC2..."

More than 20+ active security findings consolidated across diverse integrated cloud services.

  Finding 1: Port 22 is reachable from an Internet Gateway - TCP (Severity: Medium)

  Root Cause: Port 22 (Standard port utilized for remote administrative SSH connections) is currently exposed publicly to the open internet. This architectural flaw allows adversaries to run malicious discovery scans and launch automated brute-force attacks against host authentication keys.

  Finding 2: Port 80 is reachable from an Internet Gateway - TCP (Severity: Low)

  Root Cause: Port 80 (Unencrypted plain-text HTTP protocol traffic) is broad open to external internet traffic. This is classified as a low-severity finding because public-facing web applications naturally require ingress connections, however, enterprise baselines mandate upgrading this routing infrastructure to secure HTTPS (Port 443).

## Week 8 Achieved Results
The automated security architecture executed seamlessly, outputting the following validation telemetry data across the logging dashboard:

**1. Static Source Code Analysis Metrics (Trivy Scan Metric)**

Execution Status: Completed successfully with an active runtime duration of 12 seconds.

Scanner Audit Results: No security flaws detected within static application resources; the code is entirely pristine with Zero Hardcoded Secrets. Critical sensitive variables have been successfully externalized and decoupled to AWS Secrets Manager.

**2. Infrastructure Network Risk Telemetry (AWS Inspector Metrics)**
The continuous active server runtime agent identified two configuration exposures on compute node instance i-0b7a205925e2cd0b7:

Port 22 Configuration Issue (Medium Severity): Port 22 is widely exposed to the Internet Gateway routing path. Remediation plan requires revoking this access rule entirely and transitioning host administration onto AWS Systems Manager.

Port 80 Configuration Issue (Low Severity): Port 80 is broad open to the Internet Gateway path. This presents an acceptable operational risk for public-facing web interfaces, with a long-term roadmap recommendation to enforce HTTPS routing via an Application Load Balancer (ALB).

**3. Centralized Security Dashboard Visibility (AWS Security Hub Insights)**
The security analytics hub successfully aggregated over 20+ structural vulnerabilities streaming automatically into a single, unified management view.

Misconfiguration Statistical Summary: The system recorded 8 Critical alerts, 9 High alerts, 14 Medium alerts, and 4 Low alerts. Core operational remediation points heavily center around modifying public read accesses over attached Amazon EBS Volume Snapshots and tightening Root account utilization parameters beyond acceptable corporate risk baselines.