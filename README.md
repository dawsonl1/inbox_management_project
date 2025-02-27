# AI-Driven Lead Evaluation System

## Overview

This AI-driven system automates the evaluation of inbound email responses from our outreach campaigns. It assesses potential acquisition targets by analyzing **website content, business model, and product positioning**, going beyond just the data available on Grata. This solution uses **Python for the backend** and is designed to be **scalable, maintainable, and easily integratable** with our current tech stack.

---

## Architecture Overview

The system uses a **modular, Python-based architecture** divided into the following components:

1. **Inbound Email Handler** – Extracts sender domain from inbound emails.
2. **Web Scraper** – Collects key website data for analysis.
3. **AI Evaluation Engine** – Classifies and scores companies using GPT-4 Turbo.
4. **Data Enrichment Layer** – Integrates with Grata, LinkedIn, and BuiltWith for additional data.
5. **Decision Engine** – Scores leads and automates replies or flags for manual review.
6. **CRM Integration** – Logs qualified leads into Folk CRM or another CRM system.
7. **Notification System** – Notifies the team via Slack or Email for borderline cases.

---

## Tech Stack

### Backend & API Layer

- **FastAPI (Python)** – For building the backend API and business logic.
- **OpenAI Assistants API** – For AI classification and scoring.
- **Celery (Python)** – For task queuing and asynchronous processing.
- **Docker** – For containerization and easy deployment.
- **Nginx** – As a reverse proxy for handling API requests.

### Data Collection & Enrichment

- **BeautifulSoup/Scrapy (Python)** – For web scraping.
- **Grata API / LinkedIn API / BuiltWith API** – For structured company data.
- **Google's PageSpeed API** – To analyze website performance.

### Database & Storage

- **PostgreSQL (Cloud-hosted)** – For storing lead data, evaluations, and scores.
- **SQLAlchemy (Python ORM)** – For database management and queries.
- **Redis** – For caching and Celery task queue management.

### Communication & Notifications

- **Slack API (Python SDK)** – To notify the team for manual reviews.
- **SendGrid API (Python SDK)** – For automated email responses.

### CRM Integration

- **Folk CRM API** – To log qualified leads.
- **Zapier / Make.com** – To automate CRM data flow.

---

## Architecture Diagram

``` text
                       ┌───────────────────--───┐
                       │  Inbound Email Handler │
                       │   (FastAPI + Celery)   │
                       └────────┬───────────────┘
                                │ Extract Domain
                                ▼
                       ┌───────────-───────────┐
                       │     Web Scraper       │
                       │ (BeautifulSoup/Scrapy)│
                       └────────┬──────────────┘
                                │ Scraped Data
                                ▼
                       ┌────────────────--──────┐
                       │  Data Enrichment Layer │
                       │ (Grata + LinkedIn API) │
                       └────────┬───────────────┘
                                │ Enriched Data
                                ▼
                       ┌──-────────────────────-┐
                       │  AI Evaluation Engine  │
                       │ (OpenAI Assistants API)│
                       └────────┬───────────────┘
                                │ Scored Data
                                ▼
                       ┌──────────────────────-┐
                       │     Decision Engine   │
                       │   (FastAPI + Celery)  │
                       └────────┬──────────────┘
                                │ Decision
              ┌─────────────────┼───────────────-──-┐
              │                 │                   │
   ┌──────────▼─────────┐ ┌─────▼──────────-┐ ┌─────▼──────────────┐
   │ Automated Response │ │ CRM Integration │ │ Notification System│
   │  (SendGrid API)    │ │  (Folk CRM API) │ │    (Slack API)     │
   └────────────────────┘ └─────────────────┘ └────────────────────┘
```

---

## Module Breakdown & Functionality

### A. Inbound Email Handler

- **Purpose:** Extracts sender domain from inbound emails.
- **Tech:** FastAPI + Celery + Gmail API (Python)
- **Functionality:**
  - Polls inbound emails for responses.
  - Extracts the **sender's domain** (e.g., `founder@company.com → company.com`).
  - Triggers the **Web Scraper** module using **Celery** for asynchronous processing.

### B. Web Scraper

- **Purpose:** Collects key data from the company's website.
- **Tech:** BeautifulSoup/Scrapy (Python)
- **Functionality:**
  - Scrapes the **homepage, product pages, and about page**.
  - Extracts key content like **branding, product descriptions, and pricing models**.
  - Analyzes **website design and UX** using **Google's PageSpeed API**.

### C. Data Enrichment Layer

- **Purpose:** Enhances the scraped data with additional insights.
- **Tech:** Grata API / LinkedIn API / BuiltWith API (Python SDKs)
- **Functionality:**
  - Pulls **structured company data**: Employee count, revenue, funding history.
  - Identifies the **tech stack** using **BuiltWith**.

### D. AI Evaluation Engine

- **Purpose:** Classifies and scores companies.
- **Tech:** OpenAI Assistants API (GPT-4 Turbo)
- **Functionality:**
  - **Classifies** the company as **SaaS vs. non-SaaS** and **B2B vs. B2C**.
  - Evaluates **Website Professionalism**, **Mission-Critical Software**, and **Market Application**.
  - Calculates a **score out of 25** and stores in **PostgreSQL**.

### E. Decision Engine

- **Purpose:** Automates the decision-making process.
- **Tech:** FastAPI + Celery (Python)
- **Functionality:**
  - Applies decision logic based on **AI scores**.
  - Triggers actions for **automated responses, CRM logging, or manual review notifications**.

### F. Automated Response Module

- **Purpose:** Sends personalized replies.
- **Tech:** SendGrid API (Python SDK)
- **Functionality:**
  - Sends a **personalized Calendly link** if the company is a good fit.

---

## Deployment & CI/CD

- **Docker Compose** – For containerization of all services (FastAPI, Celery, PostgreSQL).
- **GitHub Actions / CircleCI** – For CI/CD pipeline.
- **AWS ECS / GCP Cloud Run** – For serverless deployment.

---

## Next Steps

1. **Build a Proof of Concept** for the **Inbound Email Handler** and **AI Evaluation Engine**.
2. **Integrate with Grata and LinkedIn APIs** for enhanced data.
3. **Deploy the MVP using Docker and FastAPI**.
4. **Test and fine-tune** the scoring and decision logic.

---

This architecture allows for **scalable, maintainable, and efficient automation** of the lead evaluation process, maximizing productivity and enabling better decision-making.  
