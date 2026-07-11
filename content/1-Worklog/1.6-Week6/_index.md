---
title: "Week 6 Blog: Automating Isolated Network (VPC) and Web Server (EC2) Deployment on AWS Using Terraform Modules"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
## Week 6 Objectives:
This week focuses on implementing the **Infrastructure as Code (IaC)** model, transitioning from manual configuration via the AWS Management Console to fully automating the provisioning, management, and deployment of cloud infrastructure using **HashiCorp Terraform**:

1. **Mastering IaC Core Concepts:** Gain a deep understanding of HCL (HashiCorp Configuration Language) syntax structure, how the AWS Provider operates to establish secure connections, and the lifecycle management of cloud resources.
2. **Modular Infrastructure Design:** Build an architectural mindset focused on code reusability by packaging independent system components into standardized Terraform Modules.
3. **Cloud Infrastructure State Management:** Enforce security and best practices for managing the `terraform.tfstate` file, understanding the State Locking mechanism, and the critical importance of centralized state storage (Remote Backend).
4. **Automated Real-World Resource Provisioning:** Practice writing code to deploy core networking components (VPC, Subnets, Route Tables) and compute resources (EC2, Security Groups) according to enterprise security standards.
5. **Documentation Optimization & Packaging (IaC Documentation):** Consolidate source code, clean up project directory structures, visualize resource workflows managed by IaC, and publish a professional report on the Technical Blog.

---

## Week 6 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :--- | :--- | :--- | :--- | :--- |
| **26/05** | Introduction to IaC & Terraform: Understand the Infrastructure as Code mindset | 26/05/2026 | 26/05/2026 | [Terraform Intro Docs](https://developer.hashicorp.com/terraform/intro) |
| **27/05** | Terraform & AWS Provider: Configure and connect AWS accounts | 27/05/2026 | 27/05/2026 | [AWS Provider Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| **28/05** | Reusable Code with Terraform Modules: Design modular infrastructure | 28/05/2026 | 28/05/2026 | [Terraform Modules Guide](https://developer.hashicorp.com/terraform/tutorials/modules/module) |
| **29/05** | Terraform State Management: Secure and manage system state files | 29/05/2026 | 29/05/2026 | [Terraform State Docs](https://developer.hashicorp.com/terraform/language/state) |
| **30/05** | Provisioning Network Infrastructure: Automate AWS VPC deployment | 30/05/2026 | 30/05/2026 | [Terraform AWS VPC Tutorial](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build) |
| **31/05** | Automating Compute & Security: Deploy EC2 and Security Groups via code | 31/05/2026 | 31/05/2026 | [Terraform AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance) |
| **01/06** | Documentation, Code Clean-up & Blog Writing: Consolidate source code and compile report | 01/06/2026 | 01/06/2026 | Hugo Portfolio Blog |

---
### Technical Evidence:
## 1. Automating Isolated Network (VPC) and Web Server (EC2) Deployment on AWS Using Terraform Modules.

**System Architecture Diagram**
![](/images/mohinh.png)
---

## 2. Lab Objectives
* **Master Infrastructure as Code (IaC):** Completely replace manual configurations by defining infrastructure using Declarative source code.
* **Modular Design Mindset:** Learn to decompose a system into separate independent blocks (`vpc` and `ec2`) to increase code reusability.
* **State Management:** Understand how `terraform.tfstate` maps declarative source code to real-world resources on AWS Cloud.
* **Real-World Troubleshooting:** Experience and troubleshoot errors related to region synchronization, state cache locks, and AMI ID formats.

---

## 3. Component Directory & Use Cases

This lab is optimized into a streamlined model to focus entirely on data workflows between Modules:

### Project Directory Structure

    terraform-lab6/
        ├── main.tf                 # Main configuration file connecting the Modules
        ├── provider.tf             # Defines the AWS Provider and working region (Singapore)
        ├── modules/                # Directory isolating distinct resource blocks
        │   ├── vpc/
        │   │   ├── main.tf         # Network definitions: VPC, Subnet, Internet Gateway, Route Table
        │   │   └── outputs.tf      # Exports vpc_id and subnet_id variables
        │   └── ec2/
        │       ├── main.tf         # Defines EC2 instance and secure Security Groups
        │       └── variables.tf    # Declares input variables received from the VPC module

Resource Component Directory
Below is the list of AWS resources initialized and managed completely automatically through Terraform source code in this practical lab

| Resource | Technical Role & Use Case |
| :--- | :--- |
| **AWS Provider** | Connects Terraform CLI with AWS APIs in the ap-southeast-1 (Singapore) region. |
| **VPC & Subnet** | Establishes an isolated virtual network partition and segments the Public Subnet serving the web server. |
| **Internet Gateway & Route Table** | Grants authorization and routes traffic to allow the server to connect to the external Internet. |
| **Security Group** | Acts as a virtual firewall instance-level defense, opening only essential ports such as 22 (SSH) and 80 (HTTP). |
| **EC2 Instance** | A virtual server running Ubuntu Server OS to host and operate applications. |

4. Step-by-Step Implementation Guide

### Step 1: Initialize Authentication Credentials

Before writing code, configure the AWS CLI on your local machine to grant secure programmatic access to Terraform:

    aws configure

### Step 2: Set Up Provider Configuration (provider.tf)
Declare the cloud service provider and the required configuration version.

    Terraform
    terraform {
      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = "~> 5.0"
        }
      }
    }

    provider "aws" {
      region = "ap-southeast-1"
    }

### Step 3: Build the VPC Module (modules/vpc/)
Initialize the virtual network partition, divide the public subnet, and configure route tables connecting directly to the Internet Gateway.

### Step 4: Build a Flexible EC2 Module (modules/ec2/)
Best Practice Solution: To avoid issues caused by hardcoded AMI IDs changing over time (such as InvalidAMIID.Malformed errors), this lab uses a data block to automatically scan and look up the cleanest, latest updated Ubuntu image straight from Canonical.

    Terraform
    data "aws_ami" "ubuntu" {
      most_recent = true
      owners      = ["099720109477"] # Canonical

      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
      }

      filter {
        name   = "virtualization-type"
        values = ["hvm"]
      }
    }

    resource "aws_instance" "web" {
      ami           = data.aws_ami.ubuntu.id
      instance_type = "t2.micro"
      # Accompanying network_interface and security group configurations go here...
    }
### Step 5: Wire Up Infrastructure in the Root Configuration (main.tf)
Call the component modules and pass Input/Output Variables to link the VPC network output with the EC2 server input.

### Step 6: Execute CLI Workflows
Execute the infrastructure deployment cycle following standard GitOps/DevOps best practices:

Bash
    # Initialize the working environment and download providers/modules
    terraform init

    # Review and verify the resource provisioning plan
    terraform plan

    # Apply changes to deploy the infrastructure to AWS Cloud
    terraform apply -auto-approve

## Week 6 Outcomes Achieved

After diagnosing and resolving underlying issues regarding virtual disk image formats (InvalidAMIID.Malformed) as well as cross-region synchronization anomalies, the lab successfully fulfilled all established milestones:

The infrastructure renders correctly on the AWS Management Console: When switching the management interface to the Singapore (ap-southeast-1) region, the entire resource cluster including the internal VPC network, Internet Gateway, Security Group, and Ubuntu Server appear up and running in a healthy state.

Mastered dynamic automation workflows: Instead of assigning static string keys that easily break over time (hardcoded AMI IDs), successfully integrating the data "aws_ami" data source allows the source code to dynamically seek out the most optimized release version from the vendor, increasing code sustainability for future re-application or scaling.

Smart resource cleanup and teardown: With just a single command, the experimental sandbox infrastructure is completely and cleanly destroyed, optimizing cloud spend and ensuring effective financial cloud management:


    terraform destroy -auto-approve
