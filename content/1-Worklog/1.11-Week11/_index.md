---
title: "Weekly 11 Blog: Finalizing Capstone Project & Evaluating Architecture via AWS Well-Architected Framework"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
## Week 11 Objectives:
This week focuses on consolidating all accumulated knowledge to finalize the Capstone Project, while applying the AWS Well-Architected Framework to review, optimize, and prepare professional technical documentation for the final project defense:

1. **Finalizing the Capstone Project:** Realize a complete cloud infrastructure system spanning networking, computing, containerization, and storage, ensuring all components connect seamlessly and automatically via Infrastructure as Code (IaC).
2. **System Evaluation via AWS Well-Architected Framework:** Conduct an architectural review based on the 6 core pillars of AWS (Security, Performance Efficiency, Reliability, Cost Optimization, Operational Excellence, and Sustainability) to proactively identify risks.
3. **Report and Presentation Preparation:** Execute rigorous load-testing scenarios, complete standardized architecture diagrams, and package professional technical documentation on the Technical Blog.

---

## Week 11 Roadmap Journal

| Day | Task | Start Date | End Date | Resource / Documentation |
| :--- | :--- | :--- | :--- | :--- |
| **06/30** | Capstone Project Design: Detailed planning, architectural decomposition, and resource allocation for deployment | 06/30/2026 | 06/30/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| **07/01** | Automated Infrastructure Provisioning: Writing Terraform source code to automate the entire core infrastructure | 07/01/2026 | 07/01/2026 | [Terraform Best Practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices) |
| **07/02** | Application Deployment & Pipeline: Configuring CI/CD to push application source code to the production cloud environment | 07/02/2026 | 07/02/2026 | [AWS Whitepapers & Guides](https://aws.amazon.com/whitepapers/) |
| **07/03** | Performance Testing & Stress Test: Utilizing load testing tools to verify fault tolerance thresholds and auto-scaling | 07/03/2026 | 07/03/2026 | [AWS Fault Injection Service](https://docs.aws.amazon.com/fis/latest/userguide/what-is-fis.html) |
| **07/04** | AWS Well-Architected Review: Reviewing the entire system using specialized AWS tools | 07/04/2026 | 07/04/2026 | [AWS Well-Architected Tool Docs](https://docs.aws.amazon.com/wellarchitected/latest/userguide/intro.html) |
| **07/05** | Cost & Security Optimization: Tuning resource sizing and tightening security policies post-evaluation | 07/05/2026 | 07/05/2026 | [AWS Cost Optimization Guide](https://aws.amazon.com/aws-cost-management/cost-optimization/) |
| **07/06** | Project Demo & Final Documentation: Finalizing the video demo, architecture diagrams, and publishing the summary report | 07/06/2026 | 07/06/2026 | Hugo Portfolio Blog |

### Real-world Evidence:

## Implementing High Availability and Auto Scaling Infrastructure on AWS using Terraform

**System Architecture Model**
![](/images/mohinh.png)

---

## 1. Lab Objectives

* **Infrastructure as Code (IaC):** Initialize and manage all Cloud system resources through automated, consistent Terraform configurations.
* **High Availability (HA):** Design a Multi-AZ network infrastructure distributed across `ap-southeast-1a` and `ap-southeast-1b` availability zones in Singapore.
* **Load Balancing & Self-Healing:** Utilize an Application Load Balancer (ALB) to distribute incoming traffic and establish an Auto Scaling Group to maintain stability and automatically replace unhealthy servers.
* **Cost and Performance Optimization (Auto Scaling Policy):** Configure a Target Tracking Scaling policy based on average CPU utilization to enable dynamic and flexible system scaling according to real-time traffic demand.

---

## 2. Components and Use Cases

| Resource Component | AWS Service Type | Specific Use Case in the System |
| :--- | :--- | :--- |
| **VPC & Internet Gateway** | `aws_vpc` / `aws_internet_gateway` | Initializes an isolated network environment with a `10.0.0.0/16` CIDR block, granting routing permissions for public internet connectivity. |
| **Public Subnets** | `aws_subnet` (1 & 2) | Splits two independent network segments into geographically isolated Availability Zones (`1a` & `1b`) to host the ALB and EC2 instances collectively for accelerated public routing. |
| **Security Groups** | `aws_security_group` | Establishes a 2-layer firewall: Layer 1 (`alb_sg`) opens public port 80 for end-users. Layer 2 (`ec2_sg`) opens secure port 80 (only accepting forwarded data from the ALB) and port 22 (SSH) for technical monitoring. |
| **Application Load Balancer** | `aws_lb` / `aws_lb_target_group` | Receives and distributes user HTTP traffic (Port 80), performing periodic physical health checks (`health_check`) to maintain healthy connections. |
| **Launch Template** | `aws_launch_template` | Defines the server template: Uses Amazon Linux 2023 OS (`ami-086f3cc9291780242`), configures `t2.micro` resources, and integrates a User Data script to automatically launch the Apache Web Server (`httpd`) and stress-test tools upon initialization. |
| **Auto Scaling Group** | `aws_autoscaling_group` | Enforces operational server counts (`min=2`, `max=4`, `desired=2`). Strictly monitors the real-time state to execute automatic scale-in/scale-out node operations. |
| **Target Tracking Scaling Policy** | `aws_autoscaling_policy` | Configures the alarm threshold. If the average CPU usage (`ASGAverageCPUUtilization`) exceeds **50.0%**, the system immediately triggers a scale-out command to provision additional servers. |

---

## 3. Detailed Step-by-Step Deployment Guide

### STEP 1: Prepare a Clean Source Code Configuration File (main.tf)

Open the CMD application on your computer. Navigate to the lab directory on the Desktop:

        cd C:\Users\matha\Desktop\aws-capstone-lab

Open the configuration file with Notepad to check or update:

        notepad main.tf

Press Ctrl + A to clear all old content inside and paste the entire standardized block of code below:

        terraform {
        required_providers {
            aws = {
            source  = "hashicorp/aws"
            version = "~> 5.0"
            }
        }
        }

        provider "aws" {
        region = "ap-southeast-1" # Singapore Region
        }

        # 1. Initialize Public VPC Network & Internet Gateway
        resource "aws_vpc" "capstone_vpc" {
        cidr_block           = "10.0.0.0/16"
        enable_dns_hostnames = true
        tags                 = { Name = "capstone-vpc" }
        }

        resource "aws_internet_gateway" "igw" {
        vpc_id = aws_vpc.capstone_vpc.id
        tags   = { Name = "capstone-igw" }
        }

        # 2. Two Subnets for High Availability (Zone 1a and 1b)
        resource "aws_subnet" "public_1" {
        vpc_id                  = aws_vpc.capstone_vpc.id
        cidr_block              = "10.0.1.0/24"
        availability_zone       = "ap-southeast-1a"
        map_public_ip_on_launch = true
        tags                    = { Name = "public-subnet-1" }
        }

        resource "aws_subnet" "public_2" {
        vpc_id                  = aws_vpc.capstone_vpc.id
        cidr_block              = "10.0.2.0/24"
        availability_zone       = "ap-southeast-1b"
        map_public_ip_on_launch = true
        tags                    = { Name = "public-subnet-2" }
        }

        # 3. Configure Route Table for Public Internet Access
        resource "aws_route_table" "public_rt" {
        vpc_id = aws_vpc.capstone_vpc.id
        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.igw.id
        }
        tags = { Name = "public-route-table" }
        }

        resource "aws_route_table_association" "pub1" {
        subnet_id      = aws_subnet.public_1.id
        route_table_id = aws_route_table.public_rt.id
        }

        resource "aws_route_table_association" "pub2" {
        subnet_id      = aws_subnet.public_2.id
        route_table_id = aws_route_table.public_rt.id
        }

        # 4. Security Group opening public port 80 for Application Load Balancer (ALB)
        resource "aws_security_group" "alb_sg" {
        name        = "alb-security-group-v2"
        vpc_id      = aws_vpc.capstone_vpc.id
        ingress {
            from_port   = 80
            to_port     = 80
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        egress {
            from_port   = 0
            to_port     = 0
            protocol    = "-1"
            cidr_blocks = ["0.0.0.0/0"]
        }
        }

        # 5. Secure Security Group for EC2 (Receives port 80 from ALB and opens port 22 for SSH)
        resource "aws_security_group" "ec2_sg" {
        name        = "ec2-security-group-v2"
        vpc_id      = aws_vpc.capstone_vpc.id

        ingress {
            from_port       = 80
            to_port         = 80
            protocol        = "tcp"
            security_groups = [aws_security_group.alb_sg.id]
        }

        ingress {
            from_port   = 22
            to_port     = 22
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"] # Grants access permission for EC2 Instance Connect
        }

        egress {
            from_port   = 0
            to_port     = 0
            protocol    = "-1"
            cidr_blocks = ["0.0.0.0/0"]
        }
        }

        # 6. Establish Application Load Balancer
        resource "aws_lb" "alb" {
        name               = "capstone-alb"
        internal           = false
        load_balancer_type = "application"
        security_groups    = [aws_security_group.alb_sg.id]
        subnets            = [aws_subnet.public_1.id, aws_subnet.public_2.id]
        }

        resource "aws_lb_target_group" "tg" {
        name     = "capstone-tg"
        port     = 80
        protocol = "HTTP"
        vpc_id   = aws_vpc.capstone_vpc.id
        health_check {
            path                = "/"
            port                = "80"
            healthy_threshold   = 2
            unhealthy_threshold = 2
            timeout             = 3
            interval            = 10
        }
        }

        resource "aws_lb_listener" "http" {
        load_balancer_arn = aws_lb.alb.arn
        port              = "80"
        protocol          = "HTTP"
        default_action {
            type             = "forward"
            target_group_arn = aws_lb_target_group.tg.arn
        }
        }

        # 7. Launch Template defining Amazon Linux 2023 server & auto web server install
        resource "aws_launch_template" "app_lt" {
        name_prefix   = "capstone-app-"
        image_id      = "ami-086f3cc9291780242" # Standard AMI code of the account
        instance_type = "t2.micro"

        network_interfaces {
            associate_public_ip_address = true 
            security_groups             = [aws_security_group.ec2_sg.id]
        }

        user_data = base64encode(<<-EOF
                    #!/bin/bash
                    dnf update -y
                    dnf install -y httpd stress
                    systemctl start httpd
                    systemctl enable httpd
                    TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
                    EC2_AVAIL_ZONE=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/placement/availability-zone)
                    echo "<h1>Hello World from AZ $EC2_AVAIL_ZONE</h1>" > /var/www/html/index.html
                    EOF
        )
        }

        # 8. Configure Auto Scaling Group managing scale dynamics
        resource "aws_autoscaling_group" "asg" {
        vpc_zone_identifier = [aws_subnet.public_1.id, aws_subnet.public_2.id]
        target_group_arns   = [aws_lb_target_group.tg.arn]
        desired_capacity    = 2
        max_size            = 4
        min_size            = 2

        launch_template {
            id      = aws_launch_template.app_lt.id
            version = "$Latest"
        }

        tag {
            key                 = "Name"
            value               = "capstone-app-instance"
            propagate_at_launch = true
        }
        }

        # 9. Auto Scaling Policy adjusting dynamically based on average CPU load
        resource "aws_autoscaling_policy" "scale_out" {
        name                   = "cpu-scale-out"
        autoscaling_group_name = aws_autoscaling_group.asg.name
        policy_type            = "TargetTrackingScaling"
        target_tracking_configuration {
            predefined_metric_specification {
            predefined_metric_type = "ASGAverageCPUUtilization"
            }
            target_value = 50.0
        }
        }

        output "alb_dns_name" {
        value       = aws_lb.alb.dns_name
        description = "Verification URL"
        }

Press Ctrl + S to save and exit Notepad.

### STEP 2: Execute Terraform Commands to Launch the Lab
Return to the CMD command line interface currently in your working directory, and run the following commands sequentially:

Re-initialize the environment:

        terraform init

Check the resource deployment plan:

        terraform plan

Automate infrastructure deployment onto AWS:

        terraform apply -auto-approve
Note: Please wait about 2 to 3 minutes for the system to provision all clean resources. Once the green text `Apply complete!` appears on screen, it is successful.

### STEP 3: Verify Load Balancing Operations
1. Scroll to the bottom of the CMD output, find the line `alb_dns_name = "..."` and copy the URL string within the quotation marks.

2. Open a web browser (or an incognito tab), paste the link into the address bar to access.

Result achieved: The webpage will display a success message: `Hello World from AZ ap-southeast-1a` (or `1b`). Press F5 (Reload) several times to see traffic automatically load-balanced across both availability zones.

![](/images/lab-alb-dns-success-1.png)

![](/images/lab-alb-dns-success-2.png)

### STEP 4: Perform Load Testing (Stress Test) to Prove Auto Scaling
Log in to the AWS Web Console -> Select the EC2 service -> Go to the Instances (Running) list.

Select 1 of the 2 virtual machines named `capstone-app-instance` -> Click the Connect button at the top.

In the EC2 Instance Connect interface, click the orange Connect button to access the Linux Terminal directly (Since port 22 is open, the connection will establish immediately).

To generate load pressure, copy and paste the command to force the CPU to run at 100% capacity:

        stress --cpu 4 --timeout 400

![](/images/lab-ssh-connect.png)

Proven Results: Keep that command execution tab open. Open a new AWS tab, navigate to EC2 > Instances. Wait 2 to 3 minutes for AWS CloudWatch to synchronize the load data, and you will see the Auto Scaling Group automatically trigger the creation of 1 to 2 new virtual machines (`Pending / Initializing`) to share the system load exactly as theoretically architected!

![](/images/lab-auto-scaling-instances.png)

## Week 11 Achieved Results
4.1. Lessons Learned
Mastery of Infrastructure as Code (IaC) Skills: Understood thoroughly how to manage and decompose an entire complex network infrastructure system, physical servers, firewalls, and load balancers from manual button clicks on the Web Console into a centralized structured source code format (`main.tf`).

High Availability (HA) Design Mindset: Mastered the operation principles of a Multi-AZ architecture. Understood how the Application Load Balancer automatically coordinates and distributes user sessions to different geographical regions (`ap-southeast-1a` and `ap-southeast-1b`) to eliminate Single Points of Failure (SPOF).

Operational Mechanism of Auto Scaling Systems: Directly configured and deeply understood the interaction between CloudWatch Alarms, Scaling Policies, and Auto Scaling Groups. Learned how to establish scenarios for monitoring average CPU metrics so the system can autonomously make scale-out decisions during overloads and scale-in actions during low traffic to optimize costs.

Self-Healing Capabilities: Proven the ability of the ASG to automatically detect faulty servers and compensate with new resources, helping the application maintain 24/7 continuity without manual intervention from system engineers.

4.2. Practical Experience (Critical Troubleshooting Lessons Learned)
Strict Terraform State Management: Learned a crucial lesson that during major changes to the network file structure (such as deleting subnets or renaming resources), executing `terraform destroy` directly can easily cause network infrastructure locks (`DependencyViolation`). Always clean up dependent resources (such as RDS Databases, ENIs, ALBs) before destroying the core VPC framework.

Dependence on Operating Systems and AMI IDs: The operating system ID (AMI ID) is strictly tied to specific AWS Regions and accounts. Misconfiguring the AMI or using outdated setup commands (`yum`) on a newer OS (`Amazon Linux 2023 requires dnf`) will cause initialization scripts (`User Data`) to fail silently, resulting in `502 Bad Gateway` errors.

Consistent Coordination Between Security Groups and Management Services: When deploying servers in isolated networks, if you only focus on opening application service ports (Port 80) but forget to open the system administration port (Port 22), tools like EC2 Instance Connect will be entirely blocked (`Connection Timed Out`), hindering monitoring and load testing. Firewalls must always be designed with a minimum of 2 clear layers.

Load Calculation Latency (Cooldown & Evaluation Periods): The AWS CloudWatch monitoring system requires a periodic cycle (usually 1-3 minutes) to accumulate data and calculate the average CPU across the entire group of servers, rather than evaluating individual machines in isolation. Consequently, during stress testing, the system exhibits a specific delay before new instances appear, requiring engineers to understand this latency to configure appropriate cooldown parameters in practice.
