---
title: "Week 2 Blog: Infrastructure Expansion & Storage Governance"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---
### Week 2 Objectives:
* Explore storage services (S3) and database services (RDS).
* Master High Availability (HA) architecture using Load Balancers and Auto Scaling.
* Conduct a practical lab to deploy a fault-tolerant system.

### Detailed Work Log (Week 2):
| Day | Task | Start Date | End Date | Documentation Source |
| :--- | :--- | :--- | :--- | :--- |
| 24/04 | Research S3 (Object Storage) & Lifecycle Policies | 24/04/2026 | 24/04/2026 | [AWS Storage](https://cloudjourney.awsstudygroup.com/) |
| 25/04 | Explore RDS (Relational Database) architecture | 25/04/2026 | 25/04/2026 | [AWS Database](https://cloudjourney.awsstudygroup.com/) |
| 26/04 | Configure Elastic Load Balancer (ELB) | 26/04/2026 | 26/04/2026 | [Compute Essentials](https://cloudjourney.awsstudygroup.com/) |
| 27/04 | Set up Auto Scaling Group (ASG) | 27/04/2026 | 27/04/2026 | [Compute Essentials](https://cloudjourney.awsstudygroup.com/) |
| 28/04 | Lab: Deploy a High Availability (HA) Web Server | 28/04/2026 | 29/04/2026 | [Workshop Link](https://cloudjourney.awsstudygroup.com/) |
| 30/04 | Weekly Summary & Blog Update | 30/04/2026 | 30/04/2026 | Self-study |

---

### Hands-on Evidence:

## 1. Cloud Storage & Data Lifecycle Management (S3 & Lifecycle Policy):

**Objective:**
The primary goal is to automate data management to optimize storage costs. Instead of manual management, the system automatically classifies and processes data based on its retention period.

**Service Explanation:**

Amazon S3 (Simple Storage Service): Used to create Buckets as secure cloud storage repositories for data.

S3 Lifecycle Policy:

Transition to Standard-IA (after 30 days): Automatically moves infrequently accessed files to a lower-cost storage tier to save money.

Expiration (after 90 days): Automatically deletes old files that are no longer needed to free up space and avoid unnecessary expenses.

* **Detailed Steps:**
    1. Navigate to the S3 Console -> **Create bucket** -> Enter a name, select a Region -> Click **Create**.
    2. Inside the newly created Bucket -> select the **Management** tab -> **Create lifecycle rule**.
    3. Enter a Rule name -> Select "Apply to all objects".
    4. Configure Actions: Select *Transition to Standard-IA* (after 30 days) and *Expire objects* (after 90 days) -> **Create rule**.
This helps optimize costs automatically without requiring manual intervention.
![S3 Bucket](/images/s3bucket.png) ![Lifecycle Rule](/images/Lifecycle%20Rule.png)

## 2. Deploying Relational Database (RDS):

**Objective:**
The primary goal is to set up and connect a relational database in the cloud. Instead of manually installing and managing a database on an EC2 virtual machine, you utilize AWS managed services to ensure stability, security, and easy scalability.

**Service Explanation:**

Amazon RDS (Relational Database Service): A service that makes it easy to set up, operate, and scale a relational database on AWS.

MySQL Engine: A popular open-source relational database management system (RDBMS) selected for storing and querying data.

Compute Resource (EC2 Connection): A feature that automatically configures Security Groups to grant an EC2 instance secure, direct access to the database.

Free Tier Template: A configuration option within the AWS Free Tier to allow practicing without incurring costs.

* **Detailed Steps:**
    1. Navigate to the RDS Console -> **Create database** -> Select **Standard create**.
    2. Choose the **MySQL** engine -> **Free tier** template.
    3. Enter the *DB identifier*, *Username*, and *Password* (make sure to remember these details).
    4. **Connectivity** section: Select your EC2 instance under *Compute resource* so AWS can automatically configure the security settings -> **Create database**.

![RDS Dashboard](/images/rds.png)

## 3. Traffic Routing & High Availability (Load Balancer):
### Objective
The primary goal is to build a network infrastructure with high availability (HA) and self-healing capabilities. The system will automatically distribute incoming traffic and replace failed servers to ensure uninterrupted service availability.

**Service Explanation:**

VPC (Virtual Private Cloud): Provisions an isolated virtual network environment spanning 2 Availability Zones. This ensures that if one zone fails, the system continues to operate in the remaining zone.

Security Group (Firewall): Controls inbound traffic, allowing only HTTP traffic (port 80) into the system to protect the servers.

Launch Template: Pre-configures server templates (Operating System, web source code) to enable rapid and consistent initialization of server instances.

Application Load Balancer (ALB): Acts as a "dispatcher" that receives user requests and distributes them evenly among the backend instances.

Auto Scaling Group (ASG): Automatically adjusts the number of servers (scales up during high traffic peaks or recreates instances when existing ones fail) to maintain system stability.

---

**Deployment Process**

**Step 1: Set up the Infrastructure Foundation (VPC)**
* Navigate to the **VPC Dashboard** -> **Create VPC**.
* Select **VPC and more**.
* **Name tag:** `MinhKhanh-VPC`.
* **Availability Zones:** 2 (Crucial for HA).
* **Public subnets:** 2.
* Click **Create VPC**.

**Step 2: Set up the Firewall (Security Group)**

We use a single shared `web-sg` for both the Load Balancer and the Web Servers.
* **Security Groups** -> **Create security group**.
* **Name:** `web-sg`.
* **VPC:** `MinhKhanh-VPC`.
* **Inbound rules:** HTTP (80) -> Source: `0.0.0.0/0`.

**Step 3: Create a Launch Template (Server Blueprint)**
This acts as the "master template" for the system to automatically spin up child instances:
* **EC2** -> **Launch Templates** -> **Create launch template**.
* **Name:** `MinhKhanh-Template`.
* **AMI:** Amazon Linux 2023.
* **Security groups:** `web-sg`.
* **User Data:**

        yum update -y

        yum install -y nginx

        systemctl start nginx

        systemctl enable nginx

        echo "Welcome from $(hostname -f)" > /usr/share/nginx/html/index.html


**Step 4: Configure the Load Balancer (ALB)**

* **EC2** -> **Load Balancers** -> **Create load balancer (ALB).**

* **Name:** `MinhKhanh-ALB.`

* **Network mapping:** Select MinhKhanh-VPC and both public subnets.

* **Security groups:** Select web-sg.

* **Target group:** Create MinhKhanh-TG (Target type: Instances).

**Step 5: Automation with Auto Scaling (ASG)**

* **Auto Scaling Groups** -> **Create Auto Scaling group.**

* **Launch template:** `MinhKhanh-Template.`

* **Load balancing:** Select Attach to an existing load balancer -> Select MinhKhanh-TG.

* **Group size:** Desired: 2, Min: 1, Max: 3.
---

**Traffic Routing Results:**
The system successfully responds from different servers, proving that the Load Balancer is distributing traffic effectively.
![Web display check](images/image_24aa49.png)
![Web display check](images/image_24aa4f.png)

**Activity History:**
The system automatically detects unhealthy instances, terminates them, and provisions new ones to maintain the desired instance count.
![System History](images/image_24aa31.png)

**Instance Status:**
The system operates stably with 3 servers (including the primary instance and the instances managed by the ASG), distributed across the `us-east-1a` and `us-east-1b` zones.
![Instance List](images/image_24aa2d.png)

---

### Week 2 Achievements:
* **Storage:** Gained a clear understanding of optimizing storage costs through automation (Lifecycle Policies).
* **High Availability:** Successfully deployed a fault-tolerant architecture, ensuring continuous service uptime.
