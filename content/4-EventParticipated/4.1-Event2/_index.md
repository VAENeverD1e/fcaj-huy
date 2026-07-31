---
title: "Event 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---


# Summary Report: "Agentic AI Buildweek Hackathon"

### Event Objectives

- Provide a hands-on hackathon platform for young engineers to build Agentic AI solutions on AWS infrastructure.
- Encourage innovative mindsets, transitioning from traditional software engineering to AI Agent-driven automation.
- Foster networking opportunities, high-pressure teamwork (24-hour hackathon), and direct feedback from industry experts (AWS, VCs).

### Speakers & Key Guests

- **Joseph Marazzotta** - Head of Technology @ AWS ASEAN
- **Nguyễn Gia Hưng** - Head of Solution Architect @ AWS Vietnam (Founder @ AWS First Cloud AI Journey)
- **Winning Team Presenters**: One Team (1st Place), Signal Scout (2nd Place), Team Plan, Team 3K, Team Six Pillar.

### Key Highlights

#### Opening Address & Vision from AWS Leadership (Joseph Marazzotta)

- **Pace of Change in the AI Era**: The shift from quarterly/bi-weekly release cycles to minute-by-minute automated deployments powered by AI Agents.
- **Innovation Mindset**: Courage to challenge legacy practices and commit to lifelong learning to shape the future of technology in Vietnam and the region.

#### Project 1: KFC AI Force Agent - Conversational Ordering (One Team - 1st Place)

- **Problem & Context**: Overcoming failures of legacy chatbots (hallucinations, incorrect orders) and eliminating customer friction caused by app downloads/account registration in F&B.
- **Solution**: Multi-channel Conversational Ordering Agent (Zalo/WhatsApp) for KFC. Customers order naturally directly within chat apps without switching interfaces.
- **Technical Architecture**: Built on **AWS Bedrock Agent Core** (session memory management), AWS WAF for traffic security, combined with TinyFish/Apify for real-time menu data scraping.
- **Impact**: 75% reduction in Bedrock infrastructure costs ($0.006 per order), low end-to-end latency (3–5 seconds).

#### Project 2: Multi-Agent Strategic Intelligence Platform (Signal Scout - 2nd Place)

- **Problem & Context**: Automating the gathering and synthesis of fragmented competitor signals (financial reports, investor updates).
- **Solution**: A Multi-Agent system that automatically scrapes, aggregates, analyzes competitor strategies, predicts ROI, and provides adaptive recommendations.
- **Technical Architecture**: Supervisor A2A (Agent-to-Agent) pattern on AWS Bedrock Agent Core, Bedrock Guardrails, LangField (data quality scoring), DynamoDB, S3, Amplify.
- **Hackathon Takeaways**: Value-first problem solving, effective team conflict resolution, and delivering functional MVP demos.

#### Project 3: SA Professional AI Native Assistant (Team Plan)

- **Problem & Context**: Automating time-consuming Cloud architecture design and cost estimation workflows for Solution Architects (SAs).
- **Solution**: Accepts natural language prompts or policy documents to generate Draw.io architecture diagrams, calculate AWS service costs, and output Terraform IaC scripts for automated deployment.
- **Highlights**: Enforces internal enterprise standards/templates, ensuring consistent infrastructure output across runs.

#### Project 4: Sheper - Autonomous Crowded Monitoring (Team 3K)

- **Problem & Context**: Monitoring and regulating crowd congestion in public venues (airports, supermarkets, event venues).
- **Solution**: Processes real-time video streams from cameras to track crowd density across designated zones and automatically dispatches staff for crowd regulation.
- **Technical Architecture**: Kinesis Video Streams, AWS Fargate cluster running YOLOv11 & ByteTrack, DynamoDB, S3, integrated with Bedrock/OpenAI Agent.

#### Project 5: Adaptive Workflow Engine for AML Investigation (Team Six Pillar)

- **Problem & Context**: Reducing high False Positive rates (90–95%) in Anti-Money Laundering (AML) investigation workflows across financial institutions.
- **Solution**: A 3-Layer architecture automating suspicious transaction investigations via 3 Sub-Agents (KYC Check, Money Flow Check, Sanction Check) with Human-in-the-loop oversight.
- **Technical Architecture**: Layer 1 (Kinesis + SageMaker XGBoost), Layer 2 (Step Functions + Bedrock Multi-Agent + OpenSearch Vector KB + Double-check LLM + Guardrails), Layer 3 (Amplify Dashboard + Cognito).
- **Highlights**: Built with Enterprise Trust standards (KMS, IAM, Secret Manager, GuardDuty, X-Ray) and double-check validation to eliminate hallucinations.

### Key Takeaways

#### Agentic AI Architectural Mindset

- **Multi-Agent Pattern (Supervisor - Sub-agents)**: Decoupling specialized tasks across agents effectively manages Context Windows, minimizes hallucinations, and simplifies feature scalability.
- **Human-in-the-loop**: In sensitive domains (Finance, AML, Operations), AI Agents assist with investigation and synthesis while leaving final decisions to humans.

#### AWS Integration Techniques

- Seamlessly orchestrating **Bedrock Agent Core**, **Step Functions**, **Kinesis**, **SageMaker**, Web Scraping tools, and Vector Databases.
- Implementing Enterprise security standards (WAF, KMS, IAM, Guardrails) and observability (CloudWatch, X-Ray).

#### Hackathon Execution Skills

- **Scope Management**: Strictly controlling project scope (MVP) within 24 hours to prevent scope creep, deadline delays, or system breaking.
- **Teamwork & Conflict Resolution**: Setting aside egos, listening to teammates, and aligning toward delivering a working product.

### Applying to Work

- **Pilot Multi-Agent Workflows**: Implement Supervisor and Sub-Agent patterns to handle complex multi-step business workflows.
- **Automate IaC & Cost Estimation**: Develop AI-assisted workflows to generate Terraform scripts and estimate AWS infrastructure costs.
- **Deploy Guardrails & Validation Layers**: Integrate output controls (Bedrock Guardrails) to increase LLM reliability in enterprise applications.

### Event Experience

Participating in the **Agentic AI Buildweek Hackathon** was an intense yet highly rewarding journey:

- **High Pressure & High Energy**: Experiencing a 24-hour sprint of brainstorming, coding, debugging, and pitching under tight time constraints.
- **Practical Learning**: Observing live demos and solutions from top-performing teams, gaining multi-dimensional insights from technical design to business execution.
- **Team Spirit & Networking**: Strengthening camaraderie through late-night coding sessions and expanding networks within the AWS community.

#### Event Photos
![Image of attending event](./../../../static/images/4-EventParticipated/event3.png)