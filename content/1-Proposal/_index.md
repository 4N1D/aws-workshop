---
title : "Proposal"
date :  2025-10-03
weight : 1
chapter : false
pre : " <b> 1. </b> "
---
# MAPVIBE

**AI Dining Recommender from Prompts & Context**  
*(Find nearby food & drink venues from natural‑language prompts and real‑time context)*

---

## 1. Executive Summary

The workshop introduces an AI‑powered dining recommender that understands Vietnamese/English natural prompts (e.g., “đói quá, muốn ăn lẩu cho 4 người dưới 300k gần Thủ Đức”) and returns relevant restaurants around **Ho Chi Minh City**. The platform integrates contextual signals—location, time, weather, group size, and past searches—to personalize suggestions. Built on **AWS serverless** architecture, it emphasizes **fast response (<10s)**, **high accuracy (≥85% match satisfaction)**, and **cost optimization (<$200 for 3 months)**. Authenticated users gain benefits such as discounts or personalized offers.

---

## 2. Problem Statement

### What’s the Problem?

- Users waste time switching between Foody, Google Maps, or GrabFood to decide where to eat.
- Current platforms don’t leverage context (time, mood, weather, budget, group size).
- Lack of conversational interaction that understands natural language.

### The Solution

The system employs **AWS Bedrock (LLM)** to parse user intent, map cravings and constraints to structured filters, then retrieve and rank results from **Google Places**, **Foody/ShopeeFood**, and **Foursquare** APIs. A hybrid conversational + form‑based interface will assist users in narrowing results and discovering trending venues.

### Benefits and ROI

- **Speed**: Reduces decision time from minutes to seconds.
- **Automation**: No manual filtering or app‑switching.
- **Personalization**: Context‑aware, learning from user behavior.
- **Educational value**: Reference model for LLM prompt parsing + AWS integration.
- **Cost efficiency**: Est. ~$65/month, $200 per 3‑month MVP.
- **Break‑even**: Immediate value for demo/research; commercial potential with partnerships.

---

## 3. Solution Architecture

### Overview

Prompt + Context → **Intent Parser (LLM)** → **Structured Query** → **Multi‑provider Search** → **Rank & Cache** → **Display on Web UI** → **Feedback loop**.
![Solution Architecture](/images/architecture.png)

### AWS Services Used

| Service                   | Function                              |
|---------------------------|---------------------------------------|
| Amazon API Gateway        | Secure endpoint for web frontend      |
| AWS Lambda                | Intent parsing, provider API calls, ranking |
| Amazon DynamoDB           | Query caching (TTL)                   |
| Amazon S3                 | Logs, exports, static assets          |
| Amazon Cognito            | Auth (email/social) + reward tracking |
| AWS Amplify / CloudFront  | Host frontend                         |
| Amazon Bedrock            | LLM intent & tone analysis            |
| OpenSearch Serverless *(optional)* | Advanced ranking experiments   |

### Component Design

- **Frontend**: Web app (Next.js, bilingual VI/EN, chat + form hybrid).
- **Data Ingestion**: Prompts, context, and survey answers go through API Gateway.
- **Data Storage**: DynamoDB (cached results), S3 (assets + logs).
- **Data Processing**: Lambda executes Bedrock prompt parsing + API aggregation.
- **User Management**: Cognito (email & social login). Non‑login users limited by IP quota.
- **Output**: List/map of top places, with metrics like open‑now, rating, distance, and mood fit.

---

## 4. Technical Implementation

### Implementation Phases

| Phase | Description                                          | Duration   |
|-------|------------------------------------------------------|------------|
| 1     | Define architecture, provider APIs, Bedrock prompt schema | 2 weeks |
| 2     | Estimate cost and caching strategy                    | 1 week     |
| 3     | Build backend (Lambda + DynamoDB + Bedrock)           | 3 weeks    |
| 4     | Develop frontend (Next.js bilingual UI)               | 3 weeks    |
| 5     | Testing and optimization (<10s latency target)        | 2 weeks    |
| 6     | Launch MVP and collect user feedback                  | 1 week     |

### Technical Requirements

- **Edge Devices**: User browser or mobile browser (PWA ready).
- **Cloud**: AWS Bedrock, Lambda, API Gateway, DynamoDB, S3, Amplify, Cognito.
- **Tools & Frameworks**: Next.js (App Router), TypeScript, AWS CDK, GitHub Actions CI/CD, Google Places SDK.

---

## 5. Timeline & Milestones

| Period              | Activities                                                  |
|---------------------|-------------------------------------------------------------|
| Pre‑Workshop (Month 0) | Research HCM restaurant datasets, test APIs                 |
| Month 1             | Build backend MVP with AWS Bedrock intent parser            |
| Month 2             | Connect provider APIs, caching, frontend integration        |
| Month 3             | Launch public beta; collect feedback                        |
| Post‑Workshop        | Extend to re‑ranking ML, offline shortlist, and event‑based recommendations |

---

## 6. Budget Estimation

### Cloud Infrastructure Costs

| AWS Service           | Cost/Month (USD) | Description              |
|-----------------------|------------------|--------------------------|
| Lambda                | 15               | API + LLM logic          |
| DynamoDB              | 10               | Cached query store       |
| S3                    | 5                | Logs, static files       |
| API Gateway           | 10               | Request routing          |
| Cognito               | 5                | Auth MAU                 |
| Amplify/CloudFront    | 10               | Hosting/CDN              |
| Bedrock (LLM tokens)  | 15               | Prompt parsing           |
| **Total**             | **≈ 65/month**   | **≈ 200/3 months**       |

### External APIs

| Provider            | Plan                | Est. Monthly |
|---------------------|---------------------|--------------|
| Google Places       | Pay‑as‑you‑go       | 15           |
| Foody/ShopeeFood    | Free API (limit)    | 0            |
| Foursquare          | Developer tier      | 5            |

---

## 7. Risk Assessment

| Risk                            | Impact | Probability | Mitigation                              |
|---------------------------------|--------|-------------|----------------------------------------|
| API quota or cost overrun       | High   | Med         | Implement cache + rate limit           |
| Inaccurate LLM parsing (VN text) | Med    | Low         | Use prompt templates & post‑validation  |
| Provider API changes            | Med    | Med         | Abstract adapters                      |
| Privacy issues (location consent)| High   | Low         | Explicit consent prompts                |

**Contingency Plans:**  
Use cached results + fallback providers. Local JSON fallback for demo. Rate‑limit unauthenticated traffic.

---

## 8. Expected Outcomes

### Technical Improvements

- Conversational search (VN/EN) with <10s latency.
- Structured schema for food intent parsing.
- Unified ranking by distance, rating, and contextual fit.

### Long‑Term Value

- Expand to ML‑based re‑ranking & personalization.
- PWA shortlist for offline decisions.
- Integration with booking or delivery APIs.
- Future extension: recommend by **weather/special events** or **social mood**.

---

### Attachments / References

- **AWS Pricing Calculator link**: [TBD]
- **Architecture Diagram**: [Pending draft]
- **GitHub Repository**: [Pending setup]
- **Budget Sheet**: [In progress]