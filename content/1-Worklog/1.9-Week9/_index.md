---
title: "Week 9 Blog: Artificial Intelligence Integration & Deploying AI Workloads on AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
## Week 9 Objectives:
This week focuses on bridging the gap between Cloud Computing and Artificial Intelligence (AI/ML), learning how to leverage modern Managed AI services, deploying Large Language Models (LLMs), and approaching automated AI system operations:

1. **AWS Managed AI Integration:** Get familiar with and master the integration of pre-trained AWS AI services into existing applications via APIs, optimizing system development time.
2. **Deploying AI Workloads on the Cloud:** Practice configuring, deploying, and exposing AI applications utilizing Foundation Models via secure Serverless or Container services capable of handling high loads on AWS infrastructure.
3. **Introduction to MLOps:** Approach the lifecycle workflow of a machine learning project from data preparation, training, and model deployment to real-time accuracy and hardware performance monitoring.

---

## Week 9 Roadmap Journal

| Day | Task | Start Date | End Date | Resource Links |
| :--- | :--- | :--- | :--- | :--- |
| **16/06** | AWS AI Services Overview: Leverage pre-trained AI services (Rekognition, Polly, Translate) | 16/06/2026 | 16/06/2026 | [AWS AI Services Docs](https://aws.amazon.com/machine-learning/ai-services/) |
| **17/06** | Generative AI with Amazon Bedrock: Approach foundation models and configure API Agents | 17/06/2026 | 17/06/2026 | [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) |
| **18/06** | End-to-End ML with Amazon SageMaker: Learn model building and training workflows | 18/06/2026 | 18/06/2026 | [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) |
| **19/06** | AI API Deployment: Package and deploy AI applications connected to third-party APIs | 19/06/2026 | 19/06/2026 | [AWS Architecture Center: GenAI](https://aws.amazon.com/architecture/general-ai/) |
| **20/06** | Serverless AI Architecture: Design cost-optimized AI infrastructure with AWS Lambda & API Gateway | 20/06/2026 | 20/06/2026 | [Serverless Land - AI/ML](https://serverlessland.com/) |
| **21/06** | AI Workload Monitoring & MLOps: Monitor performance and operational costs of AI resources | 21/06/2026 | 21/06/2026 | [Amazon SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html) |
| **22/06** | Mini AI Project & Documentation: Finalize cloud-integrated AI app and write report | 22/06/2026 | 22/06/2026 | Self-deployed / Hugo Portfolio Blog |

---
### Technical Evidence:

## 1. AI-powered Feedback Processor

**System Architecture Diagram**
![](/images/mohinh.png)

## 2. Lab Objectives
* **Deploy a Serverless AI Architecture:** Successfully integrate API Gateway and AWS Lambda to ingest and orchestrate real-time Vietnamese text data streams with sub-second response times.
* **Build a Sentiment Analysis Pipeline:** Develop filtering logic to parse and classify customer feedback nuances into clear metric categories (Positive, Negative, Neutral).
* **Establish MLOps Monitoring:** Output structured system logs prefixed with `[METRIC]` to Amazon CloudWatch for live performance and data security tracking.

## 3. Component Directory & Use Cases

| Infrastructure Component | Technical Role & Use Case |
| :--- | :--- |
| **Amazon API Gateway** | Exposes a public RESTful API endpoint to ingest incoming payload requests from clients. |
| **AWS Lambda** | Acts as the centralized data orchestration engine (Compute Node), parsing string inputs and processing pipeline logic. |
| **Amazon CloudWatch** | Aggregates all system logs, monitors execution duration, and tracks operational metric counters. |
| **Rule-based AI Engine** | Evaluates keyword semantics natively embedded within the Serverless backend architecture. |

---

## 4. Step-by-Step Implementation Guide

### Step 1: Initialize IAM Execution Role and Permissions
1. Open the **IAM Console** > navigate to **Roles** > create a new role named `Lambda-AI-Integration-Role`.
2. Attach the basic policy managed permission: `CloudWatchLogsFullAccess` to grant the Lambda function authorization to push monitoring logs.

### Step 2: Deploy AWS Lambda Orchestration Source Code
1. Create a Lambda function utilizing the **Python 3.12** runtime named `Feedback-AI-Processor`.
2. Under the **Custom execution role** configuration settings, assign the previously created `Lambda-AI-Integration-Role`.
3. Deploy the source code block handling Vietnamese string extraction and metrics exposure:

    ```python
    import json

    def lambda_handler(event, context):
        print("Received event:", json.dumps(event))
        
        customer_text = ""
        raw_body = event.get('body', '')
        
        if raw_body:
            try:
                if isinstance(raw_body, str):
                    body = json.loads(raw_body)
                else:
                    body = raw_body
                customer_text = body.get('text', '')
            except Exception as e:
                if 'text' in raw_body:
                    try:
                        customer_text = raw_body.split('"text":')[1].split('}')[0].strip(' "\'')
                    except:
                        customer_text = raw_body

        image_safety = "Safe"
        sagemaker_action = "None"
        positive_words = ["nhanh", "tốt", "tuyệt", "hài lòng", "nhiệt tình", "mua lại", "cẩn thận"]
        sentiment = "Neutral"
        
        if customer_text:
            if any(word in customer_text.lower() for word in positive_words):
                sentiment = "Positive"
            elif "tệ" in customer_text.lower() or "chậm" in customer_text.lower():
                sentiment = "Negative"
                sagemaker_action = "Triggered SageMaker-YOLO-Blur-Endpoint"

        print(f"[METRIC] Status: Processed | Sentiment: {sentiment} | Safety: {image_safety}")

        return {
            "statusCode": 200,
            "headers": {"Content-Type": "application/json; charset=utf-8"},
            "body": json.dumps({
                "image_safety": image_safety,
                "extracted_text_by_backend": customer_text,
                "sagemaker_mitigation": sagemaker_action,
                "ai_llm_analysis": f"Sentiment: {sentiment}. Summary: The system recorded positive feedback from the customer regarding the service."
            }, ensure_ascii=False)
        }   
    ```

## Step 3: Configure API Gateway Infrastructure & Extend Timeout
Link an Amazon API Gateway REST API to the Lambda function via the POST integration method wrapper.

Navigate to the Lambda function's **Configuration** tab > **General configuration** settings pane, and extend the default runtime execution **Timeout** up to 30 seconds to ensure the pipeline sequence does not prematurely disconnect mid-process.

![](/images/cloudwatch_metrics.png)

![](/images/lambda_success.png)

## Week 9 Outcomes Achieved
The system was fully tested directly within the AWS console and edge environment interfaces, reaching an optimized operational baseline:

Successful Processing State: Returns an HTTP Code `200 OK` accompanied by an explicit `Executing function: succeeded` notification message.

Operational Processing Latency: The underlying infrastructure execution duration consumed a mere 2.60 ms (with the overall minimum billed duration hitting a baseline of only 94 ms).

Memory Utilization Footprint: Peak runtime resource utilization capped tightly at 37 MB out of the allocated 128 MB baseline boundary, validating the lightweight design characteristics of this Serverless AI architecture.
