---
title: "Week 7 Blog: Mastering the Container Ecosystem with Kubernetes & Amazon EKS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
## Week 7 Objectives:
This week focuses on transitioning application infrastructure into the era of Container Orchestration, getting familiar with the core concepts of **Kubernetes (K8s)**, and hands-on practicing deployment and application management on the managed-cluster service **Amazon EKS (Elastic Kubernetes Service)**:

1. **Mastering K8s Foundations (Kubernetes Architecture & Core Objects):** Deeply understand the Master/Worker Node architecture, how the Control Plane functions, and master basic objects that constitute an application such as Pods, Deployments, and Services.
2. **Building Enterprise-Grade EKS Infrastructure (Production-Ready EKS Cluster):** Approach cluster design logic on AWS, configure IAM Roles for Service Accounts (IRSA), establish optimized VPCs for EKS, and provision clusters using specialized tools like `eksctl` or Terraform.
3. **Managing Variances & Application Routing (Application Deployment & Networking):** Execute container packaging, storage configurations, configuration management (ConfigMap/Secret), and design routing mechanisms and Load Balancing for applications running inside the Cluster.
4. **System Monitoring & Operations (Kubernetes Observability):** Deploy solutions to collect Metrics, Logs, and monitor the health status of both the Cluster and each application operating in production.
5. **Realizing the Capstone Project (Kubernetes Capstone Project):** Deploy a complete Microservices project onto Amazon EKS, applying full automation techniques for auto-scaling, network-layer security, and preserving technical documentation on the Technical Blog.

---

## Week 7 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :--- | :--- | :--- | :--- | :--- |
| **02/06** | K8s Architecture & Fundamentals: Explore the overview architecture of Kubernetes | 02/06/2026 | 02/06/2026 | [Kubernetes Concepts](https://kubernetes.io/docs/concepts/) |
| **03/06** | Core Objects (Pods, Deployments, Services): Initialize and manage resource lifecycles | 03/06/2026 | 03/06/2026 | [K8s Workloads Guide](https://kubernetes.io/docs/concepts/workloads/) |
| **04/06** | Amazon EKS Overview & Architecture: Discover AWS's managed K8s service | 04/06/2026 | 04/06/2026 | [Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) |
| **05/06** | Provisioning EKS Cluster: Practice deploying an EKS Cluster on AWS | 05/06/2026 | 05/06/2026 | [EKS Cluster Creation](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html) |
| **06/06** | Deploying Application on EKS: Push microservices and expose traffic to the Internet | 06/06/2026 | 06/06/2026 | [AWS EKS App Deployment](https://docs.aws.amazon.com/eks/latest/userguide/sample-deployment.html) |
| **07/06** | EKS Monitoring & Logging: Configure cluster resource monitoring | 07/06/2026 | 07/06/2026 | [EKS Observability](https://docs.aws.amazon.com/eks/latest/userguide/eks-observe-logging.html) |
| **08/06** | Kubernetes Capstone Project & Documentation: Package the real-world project and write blog | 08/06/2026 | 08/06/2026 | Self-deployed / Hugo Portfolio Blog |

---
### Technical Evidence:

## 1. LAB Title
**Building a Fault-Tolerant and Highly Available Web System with Automated Load Balancing on Amazon EKS (Elastic Kubernetes Service)**

**System Architecture Diagram**
![](/images/mohinh.png)

---

## 2. Lab Objectives
* Understand and operate core Kubernetes components including: Pods, Deployments, and Services.
* Practice deploying and administering a real-world Kubernetes Cluster on the AWS cloud environment via the `eksctl` command-line utility.
* Configure automated load balancing (`LoadBalancer`) mechanisms to distribute network traffic out to the public Internet.
* Activate and utilize the cluster performance metrics framework (`metrics-server`) to monitor hardware resource consumption (CPU/RAM) in real time.

---

## 3. Component Directory & Use Cases

| Component | Technology / Service | Technical Role & Use Case |
| :--- | :--- | :--- |
| **Control Plane** | Amazon EKS Cluster | Acts as the centralized brain of the infrastructure to manage, coordinate, and maintain the desired state of the entire Cluster. |
| **Worker Nodes** | AWS EC2 (t3.medium) | Virtual server instances responsible for provisioning raw computing hardware to run workload instances. |
| **Workload** | Kubernetes Deployment | Manages the lifecycle, deployment status, and maintains a stable replica count for the target Nginx Pods. |
| **Application** | Pods (Nginx Container) | The lightweight hosting boundary encapsulating source code to directly process incoming user HTTP requests. |
| **Networking** | Kubernetes Service (`LoadBalancer`) | Triggers network API calls to AWS to spin up a Public Load Balancer, routing traffic from the Internet down to internal Pods. |
| **Monitoring** | Kubernetes `metrics-server` | Aggregates real-world performance metrics allowing operators to observe cluster health. |

---

## 4. Step-by-Step Implementation Guide

### Step 1: Initialize the Amazon EKS Cluster
Use the `eksctl` command-line utility on Windows CMD to provision a complete, automated infrastructure stack including a dedicated VPC, Subnets, IAM Roles, and 2 Worker Nodes:

    eksctl create cluster --name my-eks-cluster --region us-east-1 --nodegroup-name my-nodes --node-type t3.medium --nodes 2 --managed

Note: The automated environment creation process on AWS takes approximately 15-20 minutes.

Verify the operational readiness of the underlying cluster nodes:

    kubectl get nodes

### Step 2: Define and Deploy the Workload Application (Deployment)

Create an app-deployment.yaml configuration file to define a resilient web application structure consisting of 2 identical replicas running in parallel:

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-web-app
      labels:
        app: nginx
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:latest
            ports:
            - containerPort: 80
            resources:
              requests:
                cpu: "100m"
                memory: "128Mi"
              limits:
                cpu: "200m"
                memory: "256Mi"

Apply the declarative manifest onto the EKS Cluster and audit the status of the resulting compute Pods:

  kubectl apply -f app-deployment.yaml
  kubectl get pods -o wide

### Step 3: Expose the Service to the Public Internet (Service LoadBalancer)
Create a network configuration file named app-service.yaml to securely bridge your internal EKS workload with AWS's external network load balancing service:

    apiVersion: v1
    kind: Service
    metadata:
      name: nginx-service
    spec:
      type: LoadBalancer
      selector:
        app: nginx
      ports:
        - protocol: TCP
          port: 80
          targetPort: 80

Deploy the networking configuration manifest and query the public-facing DNS address:

    kubectl apply -f app-service.yaml
    kubectl get svc nginx-service

Copy the string generated inside the EXTERNAL-IP column and access it using any standard web browser to verify the output.

![](/images/web.png)

### Step 4: Establish the Cluster Metrics Aggregator (Metrics)

Install the default metrics collection components directly from the official Kubernetes repository:

    kubectl apply -f [https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml](https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml)

Due to secure TLS certificate validation constraints inherent to the sandbox testing environment, patch the runtime arguments for the metrics-server directly via Windows CMD:

    kubectl patch deployment metrics-server -n kube-system --type="json" -p="[{\"op\": \"add\", \"path\": \"/spec/template/spec/containers/0/args/-\", \"value\": \"--kubelet-insecure-tls\"}]"

Allow the cluster scheduling state to stabilize, then query the metrics endpoint:

Check the aggregated compute resource consumption of the host machines (Nodes):

    kubectl top nodes

Query the fine-grained live compute usage for individual workload components (Pods):

    kubectl top pods

### Step 5: RESOURCE CLEANUP (TEARDOWN LAB - AVOID ACCIDENTAL CHARGES)

Because Amazon EKS charges a flat hourly fee for managing the cluster Control Plane (~$0.10/hour) alongside the concurrent operational expenses of 2 EC2 t3.medium worker nodes and the associated Load Balancer, you must cleanly tear down the lab infrastructure upon completion to prevent unexpected end-of-month cloud invoices.

Execute the following final deletion command:


    eksctl delete cluster --name my-eks-cluster

## Week 7 Outcomes Achieved
Mastered Infrastructure CLI Ecosystems: Developed proficiency in executing core infrastructure operations via eksctl and governing underlying K8s manifests using kubectl straight from a Windows workstation environment.

Architected High Availability Failover Systems: Successfully orchestrated a live web system distributed across disparate Worker Nodes, proving that the overall cluster architecture maintains continuous uptime even when facing simulated failures on individual application pods.

Automated Cloud Provider Integration: Acquired a deep understanding of cloud-native infrastructure automation workflows where a simple native Kubernetes Service interfaces directly to provision enterprise Network Load Balancers inside AWS.

Enhanced Operational Observability: Successfully navigated cross-platform pipeline constraints on Windows systems, bypassing TLS verification errors to reliably expose live cluster metrics and gather precise CPU/RAM utilization data.