---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# FCAJ Community Day 2026 

## Event Overview
* **Event Name:** FCAJ Community Day (Held on Saturday, May 23, 2026).

## Purpose of the Event
* Create a collaborative space for networking, knowledge sharing, and inspiration among members of the tech community.
* Share practical knowledge, emerging trends, and real-world production experiences from experts in Cloud Computing, Artificial Intelligence (AI), DevOps, and Platform Engineering.
* Shape the mindset and essential skills required for IT professionals (especially students and interns) in response to rapid fluctuations in both local and global labor markets.

## Speaker Lineup
1. **Mr. Nguyen Gia Hung**: Solution Architect at AWS Vietnam & Founder of FCAJ.
2. **Mr. Tinh Truong**: Platform Engineer at Gotamic (Gotam X).
3. **Mr. Hai Anh**: Young Tech Speaker (23 years old) working at Pacific Vietnam.
4. **Mr. Nguyen Han Thinh**: DevOps Engineer.
5. **Ms. Uyen & Ms. Thao**: Tech Engineers and peers, representing the Winner team of the *Los H* Hackathon (Project UTMMorpho).
6. **Expert from VPBank**: Enterprise Specialist with 2 years of hands-on experience in Platform and AI deployment at VPBank.

## Featured Sessions
The event featured 6 comprehensive tech sharing sessions centering on core pillars:

* **The Market Landscape & AI Trends (Mr. Nguyen Gia Hung):** As AI significantly lowers the cost of software development, the demand for software building will skyrocket. This shift births new operational workflows, such as AI code fixing and Platform Engineering to manage massive systems.
* **The Importance of Context in AI (Mr. Tinh Truong):** Demystified "Context in AI" and addressed the *Internet Buller* phenomenon (the over-reliance on randomly pulling internet resources, which injects noise into AI models). For AI to deliver accurate outputs, engineers must feed it specific project-level context rather than stuffing overly long prompts or generic knowledge bases.
* **Amazon Q Apps & Agent (Mr. Hai Anh):** Demoed transforming raw data (such as Excel sheets) into automated BI analytics dashboards and building smart autonomous Agents capable of taking actions (e.g., summarizing meetings and automatically sending emails).
* **AWS CloudFront Flat Rate Pricing (Mr. Nguyen Han Thinh):** Architectural solutions protecting businesses from "Bill Spikes" caused by overnight DDoS attacks or sudden traffic surges. CloudFront compresses data up to 82%, reducing CPU load for backend EC2 instances while bolstering security through mTLS and VPC Origin.
* **The 36-Hour Journey to a Hackathon Victory (Ms. Uyen & Ms. Thao):** Shared the intense journey of building *UTMMorpho* (an AI application that generates responsive HTML/CSS layouts directly from images and supports drag-and-drop editing) under high fatigue and token exhaustion boundaries.
* **Enterprise-Grade Multi-Agent Systems (VPBank Expert):** Dissected the business logic of credit scoring models for startups. Showcased how to orchestrate a cluster of specialized domain Agents (Financial, Market, Team, Risk) working collaboratively under a master Orchestrator to solve complex transactions that a single isolated Agent cannot handle.

---

## Key Takeaways & Lessons Learned

### Hard Skills
* **Optimizing the Context Window:** Understood that LLMs have strict context window limitations. Engineers must condense input data payload and decouple complex monolithic tasks into separate execution branches instead of stuffing all constraints into a single prompt.
* **Mastering LLM Execution Behaviors:** Recognized that AI is intrinsically a probabilistic model. Even setting `temperature = 0` does not guarantee 100% deterministic outputs across runs due to underlying Inference Optimization techniques applied at the infrastructure layer. Therefore, downstream services must be structurally resilient, incorporating continuous verification (*testing, testing, testing*).
* **Cloud Cost Governance and Infrastructure Scaling:** Learned that utilizing predictable pricing mechanisms (like CloudFront's Flat Rate) allows enterprise financial planners to eliminate budgeting risks stemming from traffic anomalies or external security threats.
* **Implementing Production Multi-Agent Systems:** Acquired architectural knowledge on structuring multi-agent frameworks where specialized personas handle isolated business scopes under a central Orchestrator node to automate comprehensive enterprise workflows.

### Mindset & Soft Skills
* **Shifting from "Demo" to "Production-Ready":** The current market exclusively values engineers who build real production systems solving industry-specific business problems (Domain use cases), rather than basic academic demo tasks.
* **The Supremacy of Security & Compliance:** Injecting AI into strictly regulated enterprise environments (such as banking and finance) makes data loss prevention and privacy paramount. Every integration (e.g., using MCP or AI tooling clusters) must reside within strict security boundaries and maintain a proper Audit Trail.
* **Focusing on the Core Pain Point:** When managing tight timelines or competing in hackathons, trying to over-engineer a product with excessive features leads to resource starvation. The correct strategy is solving one single, practical pain point exceptionally well.
* **Understanding Stakeholder KPIs:** Discovered that the most effective way to communicate and align with cross-functional corporate teams (such as Security or Operations) is by deeply understanding their specific KPIs and technical pressures, allowing you to tailor proposals that mitigate their risks.
* **Immediate Action Over Procrastination:** Given the breakneck speed of technological evolution (where AI capabilities practically double every 4 months), delaying personal and technical skill upgrades exponentially multiplies the difficulty of staying competitive in the modern tech market.

> **Core Event Motto:** *"Building a system isn't just about making it run. It's about making it run securely, reliably, and genuinely serving the user's needs."*

---

#### Event Gallery

![](/images/1.png)

![](/images/2.png)

![](/images/3.png)

![](/images/4.png)

![](/images/5.png)
