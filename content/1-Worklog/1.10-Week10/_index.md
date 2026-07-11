---
title: "Week 10 Blog: Building Cloud-Native Applications & Establishing Observability Frameworks"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
## Week 10 Objectives:
This week focuses on mastering next-generation system architectural mindsets, building independently scalable applications, and establishing end-to-end distributed observability frameworks to guarantee optimal resilience and stability in live production environments:

1. **Constructing End-to-End Cloud-Native Applications:** Architect software systems to fully leverage the native scalability of cloud computing, decomposing monoliths into independent microservices to optimize runtime performance and ease maintenance overhead.
2. **Enhancing System Observability:** Push beyond the boundaries of traditional metrics monitoring by gathering and tightly correlating the three core pillars of observability (Metrics, Logs, and Traces), enabling operators to proactively detect and isolate runtime failures within minutes.
3. **Applying Production Operations Principles:** Implement asynchronous message-passing topologies, graceful fault handling, and optimal data streaming paths to safeguard high availability (HA) states against sudden transactional spikes.

---

## Week 10 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :--- | :--- | :--- | :--- | :--- |
| **23/06** | Cloud Native Architecture: Learn design methodologies optimized for dynamic cloud environments | 23/06/2026 | 23/06/2026 | [CNCF Cloud Native Definition](https://github.com/cncf/toc/blob/main/DEFINITION.md) |
| **24/06** | Advanced Monitoring & Logging: Configure large-scale centralized log ingestion and governance | 24/06/2026 | 24/06/2026 | [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| **25/06** | Distributed Tracing: Track lifecycle propagation routes of user requests across distributed microservices | 25/06/2026 | 25/06/2026 | [AWS X-Ray Developer Guide](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) |
| **26/06** | Event-Driven Architecture: Explore decoupled event paradigms to minimize cross-service dependencies | 26/06/2026 | 26/06/2026 | [AWS Event-Driven Architecture](https://aws.amazon.com/event-driven-architecture/) |
| **27/06** | Message Queue & Notification: Deploy reliable asynchronous communication structures via queues and topics | 27/06/2026 | 27/06/2026 | [Amazon SQS & SNS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| **28/06** | Cloud Native Mini Project: Implement an auto-scaling and fault-tolerant microservices mesh | 28/06/2026 | 28/06/2026 | Self-deployed / AWS Architecture Center |
| **29/06** | Documentation & Architecture Review: Audit network topologies and finalize reporting on the technical blog | 29/06/2026 | 29/06/2026 | Hugo Portfolio Blog |

### Technical Evidence:

## 1. Deploying an Event-Driven Movie Ticketing Microservices Architecture on AWS ECS Fargate Integrated with Distributed Tracing & Centralized Observability

**System Architecture Diagram**
![](/images/mohinh.png)

---

## 2. Lab Objectives
* **Containerization:** Successfully package separate, independent microservices (`order-service` and `notification-service`) utilizing Docker container runtimes.
* **Orchestration & Serverless Compute:** Provision, orchestrate, and manage resilient container tasks on the cloud using **AWS ECS Fargate**, removing the need to manage underlying EC2 compute nodes.
* **Asynchronous Communication:** Establish asynchronous execution paths using an **Amazon MQ (RabbitMQ)** message broker intermediate buffer to handle API bursts and decouple processing services (Loose Coupling).
* **Distributed Tracing (Observability):** Implement distributed request lifecycle tracing by injecting a unified tracing context (`Trace ID`) flowing continuously from Client -> API -> Queue -> Worker, while structuring application telemetry outputs toward **Amazon CloudWatch Logs**.
* **Network Infrastructure & Troubleshooting:** Analyze edge network routing behaviors and debug cross-VPC networking constraints when integrating public cloud broker resources with isolated sandbox architectures.

---

## 3. Component Directory & Use Cases

| Component | Technical Role & Use Case |
| :--- | :--- |
| **Order Service (Flask API)** | Ingests incoming movie ticketing payloads from edge clients (Postman), generates a tracking context block (`Trace ID`), and publishes transactions to the message queue. |
| **Amazon MQ (RabbitMQ)** | Functions as the message broker middleware to securely buffer transactional state, enforcing strict loose coupling boundaries between the ingest API and backend workers. |
| **Notification Service (Worker)** | Runs as a persistent background daemon, polling message states out of Amazon MQ to process out-of-band notification logic (simulated confirmation emails). |
| **AWS ECR (Elastic Container Registry)** | Serves as the secure private repository to house, version, and distribute production Docker images compiled from the engineering workstation. |
| **AWS ECS Fargate** | Provides a serverless container orchestration platform, dynamically scheduling and maintaining the active healthy lifecycle configuration (`ACTIVE`) of application Tasks. |
| **Amazon CloudWatch Logs** | centralizes log aggregation across distributed application layers, speeding up root cause analysis by tracking request workflows via `Trace ID` boundaries. |

---

## 4. Step-by-Step Implementation Guide

### Step 4.1: Package and Push Application Container Artifacts to AWS ECR
1. Structure the application source directories locally and author individual `Dockerfile` configurations for each microservice boundary.
2. Initialize the local Docker daemon engine and execute programmatic credential validation against AWS API endpoints using the AWS CLI:

    ```bash
    aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.ap-southeast-1.amazonaws.com
    ```

Build, tag, and publish the compiled container images to their respective target repositories hosted inside AWS ECR:

    # Packaging the Order Service Boundary
    cd order-service && docker build -t order-service .
    docker tag order-service:latest <aws_account_id>[.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest)
    docker push <aws_account_id>[.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest)

    # Packaging the Notification Worker Boundary
    cd ../notification-service && docker build -t notification-service .
    docker tag notification-service:latest <aws_account_id>[.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest)
    docker push <aws_account_id>[.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest)

![](/images/hinh1.png)

![](/images/hinh2.png)

### Step 4.2: Construct Task Definitions and Launch Cluster Configurations on ECS Fargate
Create a serverless ECS Cluster designated as cinema-cluster operating atop automated AWS Fargate compute engines.

Author two separate infrastructure blueprints (Task Definitions): order-task mapping container egress port 5000 to public pathways, and notification-task operating as an isolated non-ingress execution daemon. Explicitly declare target Image URIs linking to ECR to grant Fargate orchestration engines deployment fetching authorization.

Establish and activate two persistent backend service engines (Services): order-api-service (mapped directly across Public Subnets with Public IP mappings enabled to receive client traffic) and notification-worker-service.

Week 10 Outcomes Achieved
Network Isolation Impedance (VPC Isolation): When provisioning an Amazon MQ broker instance utilizing Public Access topology without assigning explicit custom VPC parameters, the AWS provisioning layer defaults to deploying the resource within the AWS account's Default VPC footprint. Conversely, the ECS cluster tasks run isolated within a dedicated lab VPC subnet configuration space (cloud-native-lab).

Firewall Pathway Blocks (Security Group Block): These two VPC domains operate entirely isolated from one another without immediate interior route tables peering them together. Furthermore, the default security group mapping of the Default VPC boundary initially denies all incoming traffic (Inbound traffic) attempting access on port 5671 (the secure encrypted AMQPS endpoint required by RabbitMQ). This routing mismatch triggers systemic networking timeouts when container tasks try to publish data over the public Internet.