# Comparion Architecture & Feature Specification

## 1. Experience Overview

Comparion delivers an interactive comparison journey that blends structured product data with GPT-generated insights. Users begin by exploring categories, selecting products, and then customizing how feature weightings influence the Value for Money (VFM) score. The system returns actionable summaries, transparent scoring breakdowns, and editable prices so decisions feel personalized and data-driven.

## 2. Functional Modules

### 2.1 Landing & Discovery
* Featured categories and popular comparisons surfaced from analytics.
* Global search that supports natural language queries ("best travel laptops under $1500").
* GPT-assisted autocomplete to suggest relevant categories and filters.

### 2.2 Product Catalog Service
* Maintains canonical product metadata: identifiers, brand, pricing, release info.
* Aggregates spec sheets from trusted providers and normalizes units (e.g., GHz, mAh).
* Supports fuzzy search, filtering, and pagination per category.

### 2.3 Comparison Workspace
* Dynamic grid allowing users to add/remove products.
* Attribute grouping (Performance, Display, Battery, Design, Extras) with collapsible sections.
* Inline price editing with validation and historical price context.
* GPT callouts to highlight notable differences or trade-offs.

### 2.4 Value for Money Engine
* Configurable weight schema per category with system defaults.
* Normalization pipelines to map raw specs into comparable scores (e.g., min-max scaling within category).
* Multi-criteria decision-making model (weighted sum or Analytic Hierarchy Process) for VFM output.
* Explainability layer showing contribution percentage per attribute.

### 2.5 Personalization & Persistence
* User profiles storing saved weight templates and preferred categories.
* Comparison history and shareable permalinks.
* Optional authentication via OAuth providers.

## 3. Technical Architecture

### 3.1 Frontend (Next.js)
* **State Management**: Zustand for lightweight global state (comparison set, weights).
* **UI Components**: Tailwind CSS & Headless UI for accessibility; charting via Recharts/D3 for VFM visualization.
* **API Layer**: React Query for caching product fetches and GPT responses.
* **Real-time Feedback**: WebSocket/Server-Sent Events to stream GPT analysis increments.

### 3.2 Backend Services

| Service | Stack | Responsibilities |
| --- | --- | --- |
| API Gateway | FastAPI or NestJS | Auth, rate limiting, request routing |
| Catalog Service | Python microservice | Data ingestion, spec normalization, search |
| VFM Engine | Python service | Weight management, scoring algorithms, explanations |
| GPT Orchestrator | Node/TypeScript worker | Prompt templating, guardrails, caching |

Services communicate via REST/GraphQL for client interactions and use internal message queues (e.g., RabbitMQ) for asynchronous GPT processing tasks.

### 3.3 Data & Storage
* **PostgreSQL** for relational data (products, specs, weights, user profiles).
* **Redis** for caching frequent catalog queries and GPT response snippets.
* **Blob Storage** (S3) for catalog images and marketing assets.
* **Analytics** via event pipeline (e.g., Segment -> BigQuery) for measuring engagement.

### 3.4 Integrations
* **OpenAI GPT** for natural language insights, configured with guardrails to avoid hallucinations.
* **External Product APIs** (GS1, Amazon, GSMArena) with scheduled ETL jobs.
* **Pricing Feeds** for real-time MSRP and regional adjustments.

## 4. Data Model Snapshot

```
ProductCategory
├─ id (UUID)
├─ name
└─ weight_schema_id → WeightSchema

Product
├─ id (UUID)
├─ category_id → ProductCategory
├─ name, brand, release_date
├─ price_usd, price_currency
└─ specs JSONB

WeightSchema
├─ id (UUID)
├─ category_id → ProductCategory
├─ name ("Default", "Mobile Gaming")
└─ weights JSONB { attribute_key: weight }

ComparisonSession
├─ id (UUID)
├─ user_id → User
├─ product_ids UUID[]
├─ adjusted_prices JSONB
└─ vfm_results JSONB
```

## 5. VFM Scoring Algorithm (Draft)

1. **Normalize** each product attribute score using category-specific min/max or expert ranges.
2. **Apply Weights** from the active schema: `weighted_score = normalized_value * weight`.
3. **Aggregate** weighted scores for each product to produce a baseline VFM score.
4. **Adjust** for price by dividing by normalized cost (higher price reduces VFM).
5. **Scale** final scores to 0–10. Provide explanations referencing top contributing attributes.
6. **User Overrides**: When weights change, recompute scores instantly and log adjustments.

## 6. Prompt Engineering Guidelines

* Use system prompts to enforce factual grounding and cite data sources.
* Provide GPT with structured spec JSON so it can reason without hallucinating values.
* Include guardrail checks (e.g., function calling to validate numeric ranges).
* Stream responses to deliver progressive insights.

## 7. Milestone Plan

1. **MVP Foundations**
   * Stand up catalog schema and seed priority categories.
   * Implement product search & comparison grid without GPT.
   * Basic VFM calculation with default weights.

2. **AI Integration**
   * Add GPT summaries and question-answering about selected products.
   * Introduce streaming insights and safety filters.

3. **Personalization**
   * User accounts, saved weight profiles, comparison history.
   * Recommendation engine using collaborative filtering.

4. **Growth & Partnerships**
   * Affiliate integrations, price alerts, mobile app experiences.

## 8. Open Questions

* Preferred external APIs or datasets for initial catalog seeding?
* Regulatory/compliance requirements for pricing data in target regions?
* Monetization strategy (ads, affiliate links, subscriptions)?

---

This document will evolve as implementation details solidify. Contributions and feedback are encouraged via pull requests or issues.

