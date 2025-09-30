# Comparion V1

Comparion is an AI-assisted product comparison experience that helps shoppers surface the best value-for-money options for any category. The platform combines curated catalog data with GPT-powered insights so users can rapidly shortlist the right products for their needs.

## Project Goals

* Enable users to search across multiple product categories (e.g., mobile phones, laptops, smart home devices).
* Retrieve structured specifications and pricing data for two or more products selected by the user.
* Provide customizable comparison tables and insight summaries generated with the GPT API.
* Calculate a **Value for Money (VFM)** score based on feature weightings that adapt to the user’s preferences.

## High-Level Workflow

1. **Discover** – Users land on the site and can browse featured categories or search for a specific product type.
2. **Select Products** – A category-specific product catalog is presented. Users can add products to a comparison set via search or filters.
3. **Compare & Customize** – The comparison view retrieves specifications, surfaces AI-generated callouts, and displays price data (with manual override support).
4. **Value for Money** – The system computes VFM scores by combining normalized feature ratings with weightings. Users can fine-tune the weightings to reflect their own priorities.
5. **Decision Support** – Summaries, recommendations, and explanations from GPT help users choose the best fit.

## Core Features

### Catalog & Product Retrieval

* Category-driven navigation with search and filter controls.
* Fetch product metadata (name, brand, price, key specs) from a catalog service or external API.
* Support selecting a dynamic number of products (minimum of two).

### Comparison Experience

* Side-by-side specification table highlighting strengths and weaknesses.
* Editable price inputs so users can adjust for discounts or local pricing.
* GPT-powered insight cards that synthesize pros/cons and ideal use-cases based on the spec set.

### Value for Money Engine

* Weighted scoring model with category-specific default weights (e.g., processor > battery > display for phones).
* User-adjustable sliders to rebalance weights on the fly with immediate score recomputation.
* Transparent breakdown showing how each feature contributes to the final VFM rating on a 0–10 scale.

### Personalization & Persistence

* Save favorite weight profiles per category (e.g., "Mobile Gaming", "Travel Laptop").
* Export or share comparison results.

## Technical Architecture (Planned)

| Layer | Responsibilities | Suggested Tech |
| --- | --- | --- |
| Frontend | SPA for browsing, selection, comparison, weight adjustment, and visualization. | Next.js (React), Tailwind CSS, Zustand/Redux for state |
| Backend | Product catalog API, VFM scoring service, GPT orchestration, user preference storage. | FastAPI (Python) or Node.js (NestJS) |
| Data | Product specs, pricing history, weight templates. | PostgreSQL + Redis cache |
| AI Integration | Prompt templates, guardrails, streaming responses. | OpenAI GPT APIs |

## Repository Layout

```
Comparion_V1/
├── README.md
└── docs/
    └── architecture.md
```

> Additional directories (e.g., `frontend/`, `backend/`) will be added as implementation progresses.

## Getting Started (Draft)

1. Clone the repository and install dependencies (to be finalized).
2. Set environment variables for OpenAI API access and catalog data sources.
3. Run the development servers for frontend and backend components.
4. Use the comparison UI to explore categories and evaluate products.

## Roadmap Highlights

* [ ] Finalize data model and ingestion pipeline for product specs.
* [ ] Implement authentication and profile management for saved weight presets.
* [ ] Build VFM scoring service with transparent explainability.
* [ ] Integrate GPT-based summarization with prompt safety mechanisms.
* [ ] Launch beta comparison flows for priority categories (phones, laptops).

## Contributing

Contributions, bug reports, and feature requests are welcome! Please open an issue to discuss major changes before submitting a pull request.

