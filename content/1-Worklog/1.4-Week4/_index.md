---
title: "Week 4 Blog: DevOps, Container & Serverless on AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
## Week 4 Objectives

This week marks a major milestone in transitioning from traditional infrastructure management (VPC, EC2) to an **Application Modernization** mindset, focusing on the following core objectives:
1. **Mastering Container Technology:** Gaining a deep understanding of application packaging with Docker, managing images via Amazon ECR, and deploying them flexibly using Amazon ECS.
2. **DevOps-Driven Automation:** Building a complete CI/CD (Continuous Integration/Continuous Delivery) pipeline using the AWS CodeSuite toolset to eliminate manual operations and accelerate delivery cycles.
3. **Adopting a Serverless Mindset:** Breaking away from traditional server management constraints by shifting to a Serverless architecture (AWS Lambda, API Gateway, DynamoDB) that scales seamlessly based on traffic demand.
4. **Architectural Standardization:** Applying the principles of the *AWS Well-Architected Framework* to optimize costs and ensure robust security for Cloud-Native systems.

## Week 4 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :---: | :--- | :---: | :---: | :--- |
| **12/05** | Docker Fundamentals & Containerization: Understanding containers and application packaging | 12/05/2026 | 12/05/2026 | [Docker Docs](https://docs.docker.com/) |
| **13/05** | Amazon ECS & ECR Workshop: Deploying containers to AWS | 13/05/2026 | 13/05/2026 | [AWS ECS Workshop](https://aws.amazon.com/ecs/) |
| **14/05** | CI/CD Pipeline with CodePipeline & CodeBuild: Automating build and deployment | 14/05/2026 | 14/05/2026 | AWS CodeSuite |
| **15/05** | AWS Lambda & API Gateway: Introduction to Serverless Architecture | 15/05/2026 | 15/05/2026 | Serverless Lab |
| **16/05** | DynamoDB & S3 Integration: Building a basic serverless backend | 16/05/2026 | 16/05/2026 | AWS NoSQL |
| **17/05** | Cost Optimization & Well-Architected Review: Optimizing costs and evaluating architecture | 17/05/2026 | 17/05/2026 | [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/) |
| **18/05** | Final Project Integration & Technical Blog: Consolidation and system documentation | 18/05/2026 | 18/05/2026 | Self-study |

---

### Practical Evidence:
### 1. Docker Fundamentals & Containerization

**System Architecture Model**
![](/images/mohinh.png)

**1. Lab Objectives**

This hands-on lab establishes the core mindset of **Containerization**, eliminating environment dependencies on local physical machines or traditional virtualization to achieve key goals:
- **Understand Virtualization:** Differentiate between the architectural variations of Virtual Machines (VMs) and Containers (resource optimization, kernel sharing).
- **Master the Packaging Workflow:** Become proficient in the containerized application lifecycle: *Writing configuration (Dockerfile) -> Packaging (Image) -> Storing (Registry) -> Running instance (Container)*.
- **Understand Basic Networking:** Deep dive into **Port Mapping** to route external traffic into an isolated application running inside a container.
- **Build a Solid Cloud Foundation:** Prepare the architectural mindset required to migrate systems to advanced managed container services on AWS, such as **Amazon ECS** and **Amazon EKS (Kubernetes)**.

---

**2. Components and Use Cases**

In this lab, each tool and component acts as a fundamental building block with a direct relationship to cloud infrastructure concepts:

| Component | Lab Use Case | AWS Equivalent Component |
| :--- | :--- | :--- |
| **Dockerfile** | A text file containing a collection of instructions to automate environment provisioning for the application. | **Task Definition** (In ECS) |
| **Docker Image** | A read-only blueprint containing the source code and runtime libraries needed for the application to run anywhere. | **ECR Image Layer** |
| **Local Registry** | A local private repository on the host machine to manage and distribute self-built Docker images. | **Amazon ECR** (Elastic Container Registry) |
| **Docker Container** | An isolated runtime instance where the application executes. | **ECS Task** (Running on serverless **Fargate** infrastructure) |
| **Port Mapping (`-p`)** | A NAT/Forward mechanism routing traffic from the host machine (`8080`) to the internal container port (`3000`) for web browser access. | **Application Load Balancer (ALB)** combined with **Target Group** |

---

**3. Detailed Implementation Steps**

**Step 1: Launch the Docker Desktop Environment**
1. Ensure **Docker Desktop** is installed and running on your computer (the green whale icon indicates a `Running` status).
2. Open **PowerShell** (Windows) or **Terminal** (Mac/Linux) and run the following command to verify Docker is operational:
   docker --version

**Step 2: Create the Lab Directory**
In your open PowerShell or Terminal window, copy and run each command:

    # Create a folder named docker-lab in the current drive
    mkdir docker-lab

    # Navigate into the newly created folder
    cd docker-lab

**Step 3: Create the Web Application Source Code (server.js)**
We need to create a file containing the web execution logic.

For Windows (PowerShell), run this command to generate it quickly:

    Set-Content -Path server.js -Value 'const http = require("http"); http.createServer((req, res) => { res.statusCode = 200; res.setHeader("Content-Type", "text/html; charset=utf-8"); res.end("<h1>🐳 Chao Khanh! Container hoat dong tot roi nhe!</h1>\n"); }).listen(3000, "0.0.0.0", () => { console.log("Server running on port 3000"); });'

**Step 4: Create the Packaging Blueprint (Dockerfile)**

Similarly, create a file named Dockerfile (capital D, no file extension) in the same directory to instruct Docker on how to package the app:For Windows (PowerShell):PowerShell

    Set-Content -Path Dockerfile -Value "FROM node:18-alpine`nWORKDIR /app`nCOPY server.js .`nEXPOSE 3000`nCMD [`"node`", `"server.js`"]"

**Step 5: Build the Docker Image**

In the docker-lab directory, run the following command so Docker reads the Dockerfile, downloads the base environment, and packages it into an image named my-web:Bash

    docker build -t my-web:v1 .

You will see an output indicating my-web with the v1 tag has been successfully built.

**Step 6: Run the Container and Map Ports**

Now, convert that static image into a running container while forwarding port 8080 of your computer to port 3000 of the container:

    docker run -d -p 8080:3000 --name web-container my-web:v1


Outcome: Open a web browser (Chrome/Edge/Firefox) on your computer and navigate to:👉 http://localhost:8080


![](/images/container.png)

If the screen displays the text: "🐳 Chao Khanh! Container hoat dong tot roi nhe!", you have successfully containerized your first application!

**Step 7: Push the Application to a Local Registry**

Spin up a lightweight local repository: Run a container that serves as a storage registry, operating on port 5001 of your local machine:

    docker run -d -p 5001:5000 --name my-registry registry:2
**Step 8: Retag the Image to Point to the Local Registry**

    docker tag my-web:v1 localhost:5001/my-web:v1

**Step 9: Push the Image to the Local Registry**

    docker push localhost:5001/my-web:v1

**Step 10: Verify the Registry Repository:**

    curl http://localhost:5001/v2/_catalog

![](/images/catalog.png)

### 2. Amazon ECS & ECR Infrastructure

System Architecture Model

![](/images/mohinh1.png)

1. Workshop Objectives

This advanced hands-on lab guides you through migrating your entire application infrastructure from a local environment to an enterprise-grade AWS production architecture, fulfilling several core goals:

Mastering Container Orchestration: Learn how AWS automates the management, monitoring, and scaling of multiple containers simultaneously via Amazon ECS instead of managing manual commands locally.

Adopting Serverless Container Compute: Utilize AWS Fargate to run container applications directly without spending time managing, patching, or scaling underlying EC2 virtual servers.

Designing High Availability (HA) Systems: Configure traffic distribution and automatic container replication across multiple isolated Availability Zones (AZs).

Implementing Isolation Security: Restrict public exposure by running your core application tasks inside isolated private subnets, exposing only a single, heavily secured entry point via an Application Load Balancer.

2. AWS Services Architecture & Use Cases

In this architecture, every AWS service functions as a critical link, integrating tightly within the orchestration workflow:

AWS Services Architecture & Use Cases

In this architecture, every AWS service functions as a critical link, integrating tightly within the orchestration workflow:

| AWS Service | Lab Use Case | Technological Scope |
| :--- | :--- | :--- |
| **Amazon ECR** *(Elastic Container Registry)* | Secure Docker image registry on the cloud. Replaces the local host registry to handle image versioning (`latest`). | **Private Docker Registry** |
| **Task Definition** | The technical blueprint declaring resource parameters (CPU, RAM), ECR image paths, and required port exposure to AWS. | **Infrastructure as Code (Configuration)** |
| **Amazon ECS Cluster** | The logical grouping of virtualized hardware infrastructure providing the foundation to run services and tasks. | **Container Orchestrator Engine** |
| **AWS Fargate** | Serverless compute engine for containers. Provisions underlying hardware automatically, letting engineers focus entirely on the application container. | **Serverless Compute Engine for Containers** |
| **Application Load Balancer (ALB)** | Public load balancer situated within public subnets. Receives incoming internet traffic (Port 80) and distributes it evenly to downstream containers. | **Layer 7 Routing Gateway** |
| **Target Group** | A logical routing group mapping ALB traffic to the correct container destination using specific port configurations (`3000`). | **Traffic Router Logical Group** |

---

Step-by-Step Implementation Journal

**Step 1: Initialize an Image Repository on Amazon ECR**
1. Log in to the **AWS Management Console**, search for, and select **Elastic Container Registry (ECR)**.
2. In the left menu, select **Repositories** under **Private registry** ➔ Click the **Create repository** button.
3. Configure the parameters:
   - **Visibility settings:** Default is `Private`.
   - **Repository name:** Type exactly **`my-aws-web-app`** *(Note: Do not paste the full URL into this field to avoid validation errors)*.
4. Leave all other advanced settings at default, scroll to the bottom, and click the orange **Create repository** button.

**Step 2: Authenticate AWS CLI and Push Local Image to AWS Cloud**

1. Configure AWS Credentials Locally:**
To retrieve your keys, go to the **IAM** service ➔ Navigate to **Users** ➔ Select your current User ➔ Select the **Security credentials** tab ➔ Locate **Access keys** ➔ Click **Create access key** ➔ Select **Command Line Interface (CLI)** and complete creation. Download the `.csv` file containing your `Access key ID` and `Secret access key`.

Open **PowerShell** or **Terminal** in your project directory containing `server.js` and `Dockerfile`, then run:

    aws configure

Paste your corresponding information when prompted:

AWS Access Key ID: (Your access key string)

AWS Secret Access Key: (Your secret key string)

Default region name: Enter us-east-1 (The N. Virginia region where ECR was created).

Default output format: Press Enter to skip.

2. Log in the Docker Client to your AWS ECR Registry:
Go to the my-aws-web-app repo interface on the AWS Console and click View push commands in the top right corner. Copy command #1 (the long token authentication command) and run it in PowerShell:

    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 210347900763.dkr.ecr.us-east-1.amazonaws.com

3. Package, Tag, and Push the Image to the Cloud:
Execute the remaining three push commands sequentially (replace the account ID placeholder with your actual AWS account ID):

  Build the Docker image locally

      docker build -t my-aws-web-app .

  Tag the image to point to your remote AWS ECR repository URL

    docker tag my-aws-web-app:latest [210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest](https://210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest)

  Push the image layers up to Amazon ECR
  
    docker push [210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest](https://210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest)

Verification: Close out the instructions panel and refresh your ECR repository page. You should see an image with the latest tag and its specific file size listed.

**Step 3: Configure the Task Definition**
1. Search for ECS in the AWS Console search bar ➔ Select Elastic Container Service.
2. In the left navigation pane, choose Task Definitions ➔ Click the orange Create new task definition button (choose the UI configuration wizard).
3. Set configuration metrics:
    Task definition family: Name it web-app-task.
    Launch type: Select AWS Fargate (Serverless container mode).
    Task size: Select the smallest configuration to optimize account credits: CPU: 0.25 vCPU, Memory: 0.5 GB.
4. Configure Main Container Details:

  Name: Type web-container.

  Image URI: Paste the exact URI path of the ECR image created in Step 2 (210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest).

  Container port: Enter 3000 (The internal runtime port specified in your Node.js server.js code).

  Protocol: Leave as default HTTP.

5. Scroll to the bottom of the page and click the orange Create button.

**Step 4: Create an ECS Cluster and Deploy the Service with an Application Load Balancer (ALB)**

1. Initialize the ECS Cluster

In the ECS console, choose Clusters in the left menu ➔ Click Create cluster.

Cluster name: Set a unique name, for example: my-ecs-cluster-v2.

Infrastructure: Check only the AWS Fargate (serverless) option.

Click Create. The system will provision an empty cluster in an Active status.

2. Configure Service Deployment

Click into your new cluster my-ecs-cluster-v2 ➔ In the Services tab, click Create.

Map parameters across major sections:

  Deployment configuration:

  Compute options: Check Launch type ➔ Choose FARGATE.

  Application type: Select Service.

  Task Definition: Select the family web-app-task (created in Step 3) with the highest revision number (1 (LATEST)).

  Service name: Enter web-app-service.

  Desired tasks: Set to 2 (AWS will provision two replicated container tasks running concurrently to distribute network loads).

Networking:

VPC: Select your Default VPC.

Subnets: Select 3 subnets in different Availability Zones (us-east-1a, us-east-1b, us-east-1d). Avoid any subnets labeled with database prefixes like 'RDS-Pvt-subnet' as they lack public internet routes.

Security group: Click Create a new security group ➔ Name it web-container-sg ➔ Set Inbound rules: Type: Custom TCP, Port range: 3000, Source: Anywhere-IPv4 (0.0.0.0/0) to allow the load balancer to forward inbound data.

Load balancing:

Load balancer type: Select Application Load Balancer (ALB).

Load balancer: Select Create a new load balancer ➔ Enter my-web-alb in the blank space below.

Container to load balance: Select web-container : 3000. Click left directly on this green text block to expand hidden input fields underneath.

Listener: Select Create new listener ➔ Keep Port at default 80 (Standard HTTP protocol for client traffic) and Protocol as HTTP.

Target group: Select Create new target group ➔ Target group name: web-target-group ➔ Edit the target group Port (currently showing 80) to 3000 (so that the ALB transfers inbound port 80 traffic down into port 3000 on the containers). Keep the Health check path as /.

3. Scroll to the bottom and click the orange Create button. Wait 2-3 minutes for the system to spin up infrastructure and pull deployment metrics into a steady Running status.

**Step 5: Test Public Access via DNS**

Search for and access the EC2 service in the AWS Console ➔ Scroll the left menu to the bottom and select Load Balancers under the Load Balancing section.

Click on the newly spawned load balancer: my-web-alb.

In the Details tab at the bottom, look for the DNS name property (a long string URL ending in .amazonaws.com). Click the copy icon next to it.

Open a fresh browser tab, paste the copied DNS URL, and hit Enter (do not attach any port numbers at the tail end).

Result: The browser displays your custom welcome page, proving the container infrastructure stack is fully integrated and routed!

### 4. Building a Serverless REST API with AWS Lambda and API Gateway

System Architecture Model

![](/images/mohinhbaimoi.png)

**1. Lab Objectives**

Understand Event-Driven Models: Learn how the system triggers execution dynamically based on user HTTP requests instead of drawing continuously on idle running compute resources.

Introduction to Serverless Backends: Deploy applications and calculation logic without provisioning server OS environments or handling EC2 maintenance cycles.

Automatic Scaling Capabilities: Observe how AWS manages compute instances under the hood, scaling execution concurrency from one to thousands of simultaneous requests.

System Monitoring: Harness centralized logging streams to debug code behavior and analyze execution traces.

**2. Components and Use Cases**

| Component | Lab Use Case |
| :--- | :--- |
| **AWS Lambda** | Functions as the central backend calculation hub. Runs a Python codebase to digest JSON objects handed down by API Gateway and structure response objects. |
| **AWS API Gateway** | Functions as the public HTTPS endpoint API gateway. Accepts incoming HTTP GET traffic from internet clients, proxies the event into Lambda, and returns outputs. |
| **AWS CloudWatch Logs** | Manages system log group aggregation. Automatically catches error logging metrics and runtime standard print debugging statements from Lambda execution events. |

**3. Detailed Implementation Steps**

**Step 1: Provision the AWS Lambda Function**

1. Open the AWS Console, locate and open the Lambda dashboard.

2. Click Create function using these specifications:

Choose Author from scratch.
Function name: my-serverless-api-function
Runtime: Python 3.12
Architecture: x86_64

Permissions: Keep default (Create a new role with basic Lambda permissions).

3. Click Create function.

4. In the Code tab, overwrite the default contents of lambda_function.py with this standardized snippet:

    import json
    import datetime

    def lambda_handler(event, context):
        # Log the incoming event from API Gateway to inspect inside CloudWatch
        print("Received event: " + json.dumps(event, indent=2))

        # Generate JSON payload response for the Client
        response_body = {
            "status": "Success",
            "message": "Xin chào! Bạn đã gọi AWS Lambda thành công qua API Gateway.",
            "timestamp": datetime.datetime.utcnow().isoformat() + "Z",
            "author": "Silent Guardian of The Network"
        }

        # Structure the HTTP Response according to REST API standards
        response = {
            "statusCode": 200,
            "headers": {
                "Content-Type": "application/json",
                "Access-Control-Allow-Origin": "*" # Enables CORS for frontend integration
            },
            "body": json.dumps(response_body, ensure_ascii=False)
        }

        return response

Click Deploy to publish the codebase updates.

## Step 2: Create and Configure a REST API via API Gateway

* Search for **API Gateway** from the console.
* Locate the **REST API** card (Ensure it is not the Private variation) and click **Build**.
* Configure initialization fields:
    * **Protocol:** REST
    * **Create new API:** New API
    * **API name:** MyServerlessAPI
    * **Endpoint Type:** Regional
* Click **Create API**.

### Create a New Resource:
* In the left-hand Resources folder structure tree, click the root `/` path and click **Create resource**.
* > **Crucial:** Turn **OFF** the Proxy resource toggle switch (Ensure the button remains unselected and gray; accidental activation causes greedy path catching routing errors).
* Configure the routing property:
    * **Resource name:** hello
* Click **Create resource**.

### Create a New Method:
* Select the newly generated `/hello` resource path in the tree and click **Create method**.
* Configure integrated method properties:
    * **Method type:** Select `GET`.
    * **Integration type:** Select `Lambda Function`.
    * Enable the **Lambda proxy integration** checkbox (This is required to pass the complete request context object into the Python handler).
    * **Lambda function:** Target your function name exactly: `my-serverless-api-function`.
* Click **Create method** and acknowledge the permission pop-up letting API Gateway establish invocation policies on your Lambda task.

---

**Step 3: Deploy the API to the Public Internet**

* Inside the `MyServerlessAPI` environment panel, click **Deploy API** in the top right corner.
* Define stage boundaries in the popup menu:
    * **Stage:** Choose `New stage`.
    * **Stage name:** Enter `dev` (or prod depending on environment strategy).
* Click **Deploy**. The system generates an Invoke URL public entry point structure: `https://<your-api-id>.execute-api.<region>.amazonaws.com/dev`

> 💡 **Your fully compiled verification endpoint path will be:**
> `https://<your-api-id>.execute-api.<region>.amazonaws.com/dev/hello`

![](/images/ketquabaimoi.png)
---

**Step 4: Inspect System Logs with CloudWatch**

To understand how the Serverless backend handles computation lifecycle processes via event triggers, review the CloudWatch log streaming trails.

* Locate **CloudWatch** inside the global search bar on AWS.
* Select **Logs ➔ Log groups** in the left menu dashboard.
* Query and locate the group auto-provisioned for your function path: `/aws/lambda/my-serverless-api-function`
* Look into the list of **Log streams** (Each stream tracking block represents an isolated cold start or execution container context for Lambda). Select the latest log stream at the top.

### Key log entries to analyze:

* **Trigger Activation Metric (START):**
    ```plaintext
    START RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd Version: $LATEST
    ```
    This marks the absolute timestamp where a Lambda container context was instantiated to serve the incoming API Gateway event payload.

* **Standard Out Debug Payloads (Received event):**
    ```plaintext
    Received event: { ... }
    ```
    The direct stdout output from our Python `print()` code statement. It maps the structural JSON mapping built by API Gateway out of the client request (Parameters include Request Headers, Query Strings, User-Agents, and Client IPs).

* **Resource Cost Metric Summary (END & REPORT):**
    ```plaintext
    END RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd
    REPORT RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd  Duration: 15.42 ms  Billed Duration: 16 ms  Memory Size: 128 MB  Max Memory Used: 64 MB
    ```

---

## 5. Building a Serverless Application: S3 Image Uploads & Metadata Management with DynamoDB

**System Architecture Model**

![](/images/mohinhS3.png)

### 1. Lab Objectives

* **Incorporate a Complete Serverless Solution:** Grasp how individual AWS managed services cross-communicate seamlessly without system servers.
* **Introduction to NoSQL Databases:** Create, configure primary indexes, and handle base CRUD mutations within Amazon DynamoDB tables.
* **Integrate Storage & Compute Layers:** Use AWS Lambda to parse payload arrays, extract Base64 data blocks, compile file storage objects to Amazon S3, and commit metadata rows to DynamoDB.
* **Secure API Exposure:** Expose public HTTP POST endpoints through Amazon API Gateway utilizing Lambda proxy setups.

2. Components and Use Cases
| Component | Lab Use Case |
| :--- | :--- |
| **Amazon S3** | Object Storage - Hosts incoming binary image asset files safely with decoupled cost efficiency. |
| **Amazon DynamoDB** | NoSQL Database - Holds structured transaction metadata parameters (IDs, file keys, uploader handles, URLs, and timestamps). |
| **AWS Lambda** | Serverless Compute (FaaS) - Processes application core logic: unpacks parameters, handles base64 translations, and triggers multi-service writes. |
| **Amazon API Gateway** | API Management Gateway - Manages public HTTP POST endpoints catching JSON body frames and invoking backend functions. |
| **AWS IAM Role** | Security Entitlements - Governs granular invocation roles authorizing Lambda tasks to commit assets to S3 and put row items into DynamoDB. |

3. Detailed Implementation Steps

**Step 1: Provision an Amazon S3 Bucket**
* Search for and navigate to **S3** on the AWS Management Console.
* Click **Create bucket**.
* Provide the configuration requirements:
  * **Bucket name:** Enter a globally unique identifier name string (e.g., `my-serverless-images-bucket-khanh`).
  * **AWS Region:** Target closest geographical boundaries (e.g., `ap-southeast-1` Singapore or `us-east-1` N. Virginia).
* Keep alternative options (*Block Public Access*, *encryption rules*) at their default baselines.
* Click **Create bucket** at the base of the page.

---

**Step 2: Provision an Amazon DynamoDB Table**
* Locate and load the **DynamoDB** service dashboard.
* Click **Create table**.
* Supply operational keys:
  * **Table name:** Input exactly `ImageMetadata`.
  * **Partition key:** Input `image_id` and match data types as **String**.
* Leave settings at **Default settings** inside the *Table settings* layer.
* Click **Create table**.

---

**Step 3: Create an IAM Execution Role for AWS Lambda**
* Go to the **IAM (Identity and Access Management)** workspace console.
* Go to **Roles** in the left margin tab, then click **Create role**.
* **Trusted entity type:** Select **AWS service**.
* **Service or use case:** Dropdown select **Lambda** $\rightarrow$ Click **Next**.
* Inside the *Add permissions* page, search for and check the boxes for these pre-packaged policies:
  * `AWSLambdaBasicExecutionRole` (Grants permission to push system logs to CloudWatch).
  * `AmazonS3FullAccess` (Grants data manipulation permissions over S3 resources).
  * `AmazonDynamoDBFullAccess` (Grants read/write capabilities over DynamoDB database rows).
* Click **Next**.
* **Role name:** Define a descriptive identifier handle like `Lambda-S3-DynamoDB-Role`.
* Double-check configured policies and click **Create role**.

---

**Step 4: Create and Configure the AWS Lambda Function**
* Select the **Lambda** service console space.
* Click **Create function** and map variables:
  * Keep **Author from scratch** active.
  * **Function name:** Enter `ImageUploadHandler`.
  * **Runtime:** Match **Python 3.12** (or latest Python iteration).
  * **Permissions:** Expand *Change default execution role*, toggle **Use an existing role**, and select `Lambda-S3-DynamoDB-Role` generated during Step 3.
* Open **Additional settings** at the footer (Confirm *Proxy resource* is unselected) $\rightarrow$ Click **Create function**.
* Inside the *Code source editor* field, switch out stock template text lines completely for this Python execution codebase block:

    import json
    import boto3
    import base64
    import uuid
    import os
    from datetime import datetime

    Initialize AWS SDK clients
    
    s3 = boto3.client('s3')
    dynamodb = boto3.resource('dynamodb')

   Retrieve S3 and DynamoDB configurations from Environment Variables

    BUCKET_NAME = os.environ.get('BUCKET_NAME')
    TABLE_NAME = os.environ.get('TABLE_NAME')
    table = dynamodb.Table(TABLE_NAME)

    def lambda_handler(event, context):
        try:
            # 1. Parse data coming through API Gateway Proxy Integration
            body = json.loads(event.get('body', '{}')) if isinstance(event.get('body'), str) else event.get('body', {})

            image_base64 = body.get('image_data')
            file_name = body.get('file_name', 'unnamed.jpg')
            uploader = body.get('uploader', 'anonymous')

            if not image_base64:
                return {
                    'statusCode': 400,
                    'body': json.dumps({'message': 'Missing image_data in request body'})
                }

            # 2. Decode the Base64 payload string into Binary raw asset data
            image_binary = base64.b64decode(image_base64)

            # 3. Create a unique UUID file name string pattern and build the S3 Key path
            image_id = str(uuid.uuid4())
            s3_key = f"uploads/{image_id}_{file_name}"

            # 4. Put Object to write asset file inside S3 Bucket
            s3.put_object(
                Bucket=BUCKET_NAME,
                Key=s3_key,
                Body=image_binary,
                ContentType='image/jpeg'
            )

            # Compile target structural asset image URL string for entry path routing
            image_url = f"https://{BUCKET_NAME}[.s3.amazonaws.com/](https://.s3.amazonaws.com/){s3_key}"

            # 5. Put Item to inject relational image parameter row into DynamoDB (CRUD - Create)
            timestamp = datetime.utcnow().isoformat()
            metadata_item = {
                'image_id': image_id,
                'file_name': file_name,
                'uploader': uploader,
                's3_url': image_url,
                'uploaded_at': timestamp
            }
            table.put_item(Item=metadata_item)

            # 6. Return response object to API Gateway Proxy specifications
            return {
                'statusCode': 201,
                'headers': {
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                },
                'body': json.dumps({
                    'message': 'Upload thành công!',
                    'image_id': image_id,
                    's3_url': image_url
                }, ensure_ascii=False)
            }

        except Exception as e:
            print(f"Error details: {str(e)}")
            return {
                'statusCode': 500,
                'body': json.dumps({'message': 'Internal Server Error', 'error': str(e)})
            }

5. Click Deploy to process codebase deployments.

6. Configure Operational Environment Variables:

Toggle to the Configuration submenu tab along the top horizontal function navigation layout.

Click Environment variables on the left margin tree menu list.

Click Edit and pass two required string variable assignments:
    BUCKET_NAME: Match exact bucket text created during Step 1 (e.g., my-serverless-images-bucket-khanh).
    TABLE_NAME: Input precisely ImageMetadata.

Click Save to lock environmental maps.

**Step 5: Instantiate Amazon API Gateway as the Endpoint Gate**
1. Locate and run API Gateway within the core global navigation console bar.

2. fUnder the REST API platform card (confirm not the Private variation option), click Build.

3. Choose the New API checkbox selection option and fill out configurations:

    API name: ServerlessImageAPI
    Endpoint Type: Select Regional

4. Click Create API.

Create Resource /upload

1. Inside the structural resource configuration landing zone, click Create resource.

2. ⚠️ Critical Check: Switch OFF the Proxy resource toggle switch (ensure button color reads gray) to prevent greedy path parsing errors.

3. Define the resource tracking lines:

    Resource name: Input upload
    Resource path: Auto-evaluates matching target routing as /upload

4. Enable the CORS (Cross-Origin Resource Sharing) configuration choice checkbox to clear cross-domain evaluation access failures.

5. Click Create resource.

Create Method POST and Attach Lambda Execution Targets

1. Select the generated /upload item block path in your configuration tree hierarchy, then click Create method.

2. Configure method parameters:

    Method type: Select POST
    Integration type: Check Lambda Function
    🟢 Turn ON the Lambda proxy integration toggle selector switch (The slider indicator reads green). This mapping flag is mandatory to pass complete client context objects (Headers, Bodies) to backend Python loops.
    Lambda function: Match tracking regions and search for your handler string: ImageUploadHandler.

3. Click Create method.

Deploy API Configurations Publicly

1. Click the Deploy API button option at the upper right side section menu.

2. Provide target deployment markers:

    Stage: Select *New stage*

    Stage name: Input prod

3. Click Deploy.

4. Upon successful deployment, the system displays the public Invoke URL mapping framework path structured as:

    https://[api-id].execute-api.[region][.amazonaws.com/prod](https://.amazonaws.com/prod)

![](/images/minhchungS3.png)

### Week 4 Achievements

With the completion of Week 4, my cloud engineering skill set and practical portfolio assets have seen a comprehensive upgrade:

Successful Containerization & Deployment: Mastered writing multi-stage Dockerfiles, managing secure image layers via Amazon ECR, and defining Task Definitions and Services to maintain highly available tasks inside Amazon ECS.

Automated CI/CD Pipeline Execution: Engineered a continuous integration and deployment loop using AWS CodePipeline linked with GitHub tracking branches. Every new code push triggers AWS CodeBuild tasks to build and package Docker image layers and push automated rolling updates across the ECS clusters.

Full Serverless Architecture Implementation: Built an event-driven REST API backend ecosystem: handles incoming client JSON objects via API Gateway, processes computations with AWS Lambda, retains relational transaction parameters inside Amazon DynamoDB, and hosts application assets on Amazon S3.

Cost Allocation & System Administration: Developed practical experience querying budget metrics, implementing lean optimization policies for compute environments, and composing a high-quality technical blog article to share architectural designs.