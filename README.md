# ⚡ Agentic AI for Maritime Freight Pricing and Route Optimization
### Codename: **FreightQuote AI**

*An agentic decision-support copilot for an ocean-freight brokerage — grounded routing, pricing, weather, and compliance answers.*

![Python](https://img.shields.io/badge/python-3.10%2B-blue) ![Streamlit](https://img.shields.io/badge/streamlit-1.36%2B-red) ![License](https://img.shields.io/badge/license-MIT-green)

---

## Table of Contents

1. [Program & Team Context](#program--team-context)
2. [Overall Project Explanation](#overall-project-explanation)
3. [Architecture Diagram](#architecture-diagram)
4. [The 9 Specialised Agents](#the-9-specialised-agents)
5. [Authentication, OTP & Security](#authentication-otp--security)
6. [Admin Dashboard](#admin-dashboard)
7. [Screenshots / GIFs](#screenshots--gifs)
8. [Installation & Run Instructions](#installation--run-instructions-from-github)
9. [requirements.txt](#requirementstxt)
10. [Demo Video](#demo-video)
11. [Known Limitations & Future Scope](#known-limitations--future-scope)
12. [Acknowledgements](#acknowledgements)

---

## Program & Team Context

**Infosys Springboard Internship — Batch 1**

**Mentor:** Mohammed Sipli

**Team Members:**

| Name | Role / What They Built | GitHub Handle |
|---|---|---|
| Alakshya Nilesh Salvi | Authentication module (login, registration, RBAC) and Gmail OTP integration | @AlexSalviIQ |
| Simran Kapoor | AI Copilot, LLM engine, and RAG pipeline (FAISS/BM25 hybrid retrieval, RAG Builder notebook) | @simrankapoor33 |
| Nadendla Padhvika | Freight & pricing agents (Route Intelligence, Dynamic Pricing, Margin Predictor) and analytics dashboards | — |
| S Uday Gowda | Admin Dashboard, database design/seeding, deployment (FastAPI backend, Cloudflare Tunnel), and Data Pipeline/ML Trainer notebook | — |
| Manuru Deepika | UI theme design, Multilingual Translation Studio, and documentation | @manurudeepika02-del |

---

## Overall Project Explanation

**Problem Statement:** Ocean-freight brokers and regional operations teams juggle scattered data — port congestion, carrier safety records, live weather risk, customs duty rules, and rate benchmarking — across disconnected spreadsheets and portals. Decisions on routing and pricing are slow, inconsistent, and hard to audit. FreightQuote AI gives brokers, dispatchers, and clients a single conversational and visual workspace to get grounded, data-backed answers instantly.

**Solution Summary:** FreightQuote AI is a role-aware Streamlit portal backed by a SQLite data layer and a suite of 9 specialised reasoning agents. An intent router classifies each query (shipment, pricing, weather, or customs) and hands it to the right agent, which pulls facts straight from the database. A local Qwen2.5 LLM (with automatic fallback to a smaller model) turns those retrieved facts into a natural-language answer — never inventing numbers. The result is a maritime freight copilot that is transparent about its ML models, safe with secrets, and usable by every role in the brokerage, from admin to client.

**Architecture Overview:** 

<img width="1360" height="1240" alt="architecture-diagram" src="https://github.com/user-attachments/assets/cb1e30dc-fadd-4733-8787-f30b1a25ba6a" />


**Key Differentiators:**
- **Grounded generation** — the LLM answers only from retrieved database facts; no hallucinated numbers.
- **Transparent ML** — every predictive agent shows multi-model benchmarking (RandomForest, GradientBoosting, DecisionTree, Logistic/Linear Regression, SVC) so the "best" model is chosen visibly, not silently.
- **RBAC role-awareness** — five roles (Admin, Operations Manager, Freight Broker, Dispatcher, Customer/Client) see different tabs and data.
- **Fail-soft LLM degrade path** — Qwen2.5-3B-Instruct automatically falls back to the 1.5B variant if VRAM is insufficient, so the app never hard-crashes on a smaller GPU.

---

## Architecture Diagram

> Export a real diagram (draw.io / Excalidraw / PowerPoint SmartArt) from the flow below and save it as `docs/architecture-diagram.png`, then it will render here:
>
> `![Architecture](docs/architecture-diagram.png)`

```
┌─────────────────────────────────────────────────────────────┐
│ 1. DATA LAYER                                                │
│ seed_data.py populates SQLite with ports, shipments,         │
│ carriers, routes, freight quotes, customs data, and          │
│ weather-risk snapshots.                                      │
└───────────────────────────────┬───────────────────────────────┘
                                 ▼
┌─────────────────────────────────────────────────────────────┐
│ 2. REASONING TOOLS LAYER                                     │
│ 9 agent modules covering routes, pricing, carriers, weather,  │
│ margin, customs, documents, translation, and PDF RAG.        │
└───────────────────────────────┬───────────────────────────────┘
                                 ▼
┌─────────────────────────────────────────────────────────────┐
│ 3. ORCHESTRATION LAYER                                       │
│ intent_router.py classifies queries into                     │
│ shipment/pricing/weather/customs, with a Haversine route     │
│ solver and a from-scratch freight-quote calculator.          │
└───────────────────────────────┬───────────────────────────────┘
                                 ▼
┌─────────────────────────────────────────────────────────────┐
│ 4. GENERATION LAYER                                          │
│ llm_engine.py runs Qwen2.5-3B-Instruct (fallback 1.5B) to    │
│ generate the final grounded answer from retrieved facts.     │
└─────────────────────────────────────────────────────────────┘
```

### Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| Frontend / UI | Streamlit, streamlit-option-menu, streamlit-folium | Role-aware web portal, maps, navigation |
| Data | SQLite, pandas | Ports, shipments, carriers, routes, quotes, customs, weather snapshots |
| ML Benchmarking | scikit-learn (RandomForest, GradientBoosting, DecisionTree, Logistic/Linear Regression, SVC), IsolationForest | Multi-model benchmarking per agent, anomaly detection |
| LLM / Generation | Qwen2.5-3B-Instruct (fallback 1.5B), Transformers, FastAPI microservice | Grounded natural-language answer generation |
| RAG | FAISS, sentence-transformers, rank-bm25, pdfplumber | Hybrid dense + sparse retrieval over uploaded PDFs |
| Translation | NLLB-200-distilled-600M | Offline multilingual document/SOP translation |
| Auth & Security | bcrypt, PyJWT, Gmail SMTP (App Password) | Login, registration, RBAC, OTP-based password reset |
| Visualization | Plotly, Folium | Charts, maps, dashboards |
| Reporting | ReportLab, FPDF | Freight quote PDFs, Bill of Lading generation |
| Deployment | Cloudflare Tunnel (`cloudflared`), Google Colab | Public URL for the Streamlit app when run from Colab |

---

## The 9 Specialised Agents

```
                 AI COPILOT / ORCHESTRATION LAYER
                        (intent_router.py)
            ▼ routes each query to the right agent ▼

 1. Port & Route Intel     2. Freight Pricing        3. Carrier Performance
 4. Weather & Harbor Risk  5. Margin Optimizer        6. Customs & Compliance
 7. Quote & BoL Docs       8. Document Translation     9. Document RAG Engine
```

**Glossary:** BAF = Bunker Adjustment Factor · TEU = Twenty-foot Equivalent Unit · HS Code = Harmonized System commodity code · Dwell time = time cargo/vessel spends at port · BoL = Bill of Lading.

### Agent 1 — Global Ocean Port & Route Intelligence
Telemetry and routing across monitored global and Indian ports with a live congestion map.
- **Data:** `ports`, `routes`, `shipments` tables
- **ML models benchmarked:** RandomForest, GradientBoosting, DecisionTree, LogisticRegression, SVC
- **Charts:** Folium map, Bar, Scatter
- **Output:** Congestion map, route recommendation, best-model prediction

### Agent 2 — Dynamic Freight Pricing & Rate Calculator
Calculates and benchmarks ocean freight quotes (base rate + fuel surcharge + customs/terminal fees).
- **Data:** `freight_quotes`, `routes`, `carriers` tables
- **ML models benchmarked:** RandomForestRegressor, GradientBoostingRegressor, DecisionTreeRegressor, LinearRegression
- **Charts:** Waterfall, Heatmap, Funnel, Bar
- **Output:** Rate breakdown, benchmark table, predicted quote

### Agent 3 — Carrier Performance & Safety Audit
Benchmarks shipping carriers on safety, reliability, and fleet capacity.
- **Data:** `carriers`, `shipments` tables
- **ML models benchmarked:** RandomForestClassifier, GradientBoostingClassifier, DecisionTreeClassifier, LogisticRegression, SVC
- **Charts:** Treemap, Scatter, Heatmap
- **Output:** Carrier scorecard, safety classification

### Agent 4 — Global Weather Risk & Harbor Safety Intelligence
Monitors severe-weather risk at monitored ports using live Open-Meteo data.
- **Data:** `ports`, weather-risk snapshots, live Open-Meteo API
- **ML models benchmarked:** RandomForest, GradientBoosting, DecisionTree, LogisticRegression, SVC, LinearRegression
- **Charts:** Folium map, Bar, Scatter
- **Output:** Risk map, alert level, forecast trend

### Agent 5 — Freight Margin Optimizer & Profitability Intelligence
Analyses where margin is earned or lost across quotes, with a carrier yield matrix.
- **Data:** `freight_quotes`, `carriers`, `shipments` tables
- **ML models benchmarked:** RandomForestRegressor, GradientBoostingRegressor, DecisionTreeRegressor, LinearRegression
- **Charts:** Box plot, Heatmap, Histogram
- **Output:** Margin distribution, yield matrix, predicted margin

### Agent 6 — Customs Intelligence & HS Code Compliance
Assesses regulatory clearance risk by country and cargo type with an 8-parameter duty simulator.
- **Data:** `customs` table
- **ML models benchmarked:** RandomForestClassifier, GradientBoostingClassifier, DecisionTreeClassifier, LogisticRegression, SVC
- **Charts:** Sunburst, Scatter
- **Output:** Compliance risk classification, duty simulation

### Agent 7 — Quote Document & Bill of Lading Generator (OCR)
Produces shipping paperwork — auto-generated freight quote PDF and Bill of Lading — from live quote data.
- **Data:** `freight_quotes`, `shipments` tables
- **ML models benchmarked:** Document generation (ReportLab / FPDF) — no ML benchmark
- **Output:** Downloadable PDF quote / BoL

### Agent 8 — Freight Document & Policy Translation Engine
Offline translation of freight documents and policies, plus a maritime trade glossary (BAF, TEU, HS Code, dwell time).
- **ML models benchmarked:** NLLB-200-distilled-600M — translation, not a classical ML benchmark
- **Output:** Translated document/SOP text in 20+ languages

### Agent 9 — Custom PDF Knowledge Base & Vector RAG Engine
Upload-your-own-document workbench for customs manuals and carrier contracts, chunked and indexed for grounded Q&A.
- **ML models benchmarked:** FAISS + sentence-transformers hybrid retrieval (dense) with BM25 (sparse) — no classical ML benchmark
- **Output:** Grounded answers cited from the uploaded PDF

---

## Authentication, OTP & Security

**Auth Flow:** `Signup → Login → JWT session → Forgot Password → Gmail OTP (or Security Question fallback) → Reset`

OTP delivery credentials and all secrets are configured via environment variables and are **never** committed to the repository (see [`.env.example`](#requirementstxt) and the security checklist below).

**RBAC Roles:**

| Role | Typical Access |
|---|---|
| Admin | All tabs, including the Admin Dashboard and full agent suite |
| Operations Manager | Full operational + Data Feed Center access, excluding the Admin Dashboard |
| Freight Broker | All agents and the AI Copilot, excluding the Admin Dashboard |
| Dispatcher | AI Copilot + a subset of operational agents |
| Customer / Client | AI Copilot plus quote-related agents only |

**Default Admin Login (demo/local only):**
```
Email:    admin@infosys.com
Password: admin123
```
⚠️ Change this credential before any real deployment — it is a seeded demo account, not a production secret.

---

## Admin Dashboard

*Add a screenshot of the running Admin Dashboard here, e.g. `![Admin Dashboard](docs/screenshots/admin-dashboard.png)`*

**Admin-only capabilities:**
- User management & role assignment (add / promote / demote / lock / unlock / delete)
- System health monitoring (DB status, GPU/VRAM, LLM backend status, app uptime)
- ML model performance ledger (accuracy / F1 / R² per agent)
- Chat history & audit trail across all users
- Database maintenance (VACUUM / re-seed)

---

## Screenshots / GIFs

## 1. Login Page

<img width="1920" height="1080" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/efefc0d5-adf2-4d41-bc63-73696d21b22d" />

## 2. Agent

<img width="1920" height="1080" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/aaf20500-9549-4b28-97e0-17e2d7b7d1ac" />

## 3. Admin Dashboard

<img width="1920" height="1080" alt="Screenshot (149)" src="https://github.com/user-attachments/assets/48e32f9b-93a9-41b0-a72a-f0c189764a51" />

## 4. Reset Password

<img width="1920" height="1080" alt="Screenshot (172)" src="https://github.com/user-attachments/assets/8bfe820f-362f-4115-b88c-4a75db9c6a6b" />

## 5. OTP Flow

<img width="1920" height="1080" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/fe63c005-307e-4d2b-954d-76577611dfd6" />

---

## Installation & Run Instructions (from GitHub)

```bash
# 1. Clone the repository
git clone https://github.com/manurudeepika02-del/Infosys_AgenticFreight_AI.git
cd Infosys_AgenticFreight_AI

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
cp .env.example .env
# then open .env and fill in YOUR OWN values (see Secrets & Credentials below)

# 5. Seed the database (first run only)
python seed_data.py

# 6. Run the app
streamlit run app.py
```

### Run on Google Colab

The project can also be run directly from `Maritime_Freight_Portal_Milestone4.ipynb`:

1. Open the notebook in Colab and set the runtime to **GPU** (T4 or better recommended).
2. Add `EMAIL_ID` and `EMAIL_PASSWORD` (Gmail App Password) as **Colab Secrets** — never hard-code them in a cell.
3. Run the cells top-to-bottom in order:
   - Dependency installation cells (pins `numpy`, installs `bcrypt`/`PyJWT`, FAISS/BM25 RAG stack, Transformers stack)
   - All `%%writefile freight_app/...` cells (these write every module — `app.py`, `auth.py`, `db.py`, all 9 agents, `llm_engine.py`, `rag_engine.py`, etc.)
   - **Initialize and seed database** cell
   - **Boot AI microservice backend & FastAPI server** cell (loads Qwen2.5, starts `model_server.py` on port 8000)
   - **Launch Streamlit application & Cloudflare public tunnel** cell — prints a public `https://*.trycloudflare.com` URL
4. Open the printed Cloudflare URL to use the app.

### Minimum Requirements

- **Python:** 3.10+
- **GPU/VRAM:** A CUDA GPU with ≥ 6 GB VRAM is recommended to run Qwen2.5-3B-Instruct comfortably; the app **auto-degrades to the Qwen2.5-1.5B-Instruct model** if VRAM is insufficient, so it still runs on smaller GPUs (or CPU, with slower generation).
- **Disk space:** Several GB free for LLM + NLLB translation model weights.

---

## requirements.txt

Full pinned dependency list (grouped for readability):

```
# Core
streamlit>=1.36
streamlit-option-menu>=0.3.13
streamlit-folium>=0.22
folium>=0.15

# ML
scikit-learn
numpy==2.0.2
pandas

# LLM & NLP
transformers>=4.41
torch>=2.2
sentencepiece>=0.2.0
accelerate>=0.30
bitsandbytes
faiss-cpu>=1.8.0
sentence-transformers>=3.0
rank-bm25>=0.2.2
deep-translator>=1.11

# Auth
bcrypt>=4.0
PyJWT>=2.8

# Reporting
reportlab>=4.0
fpdf>=1.7
pdfplumber>=0.11
PyMuPDF>=1.23.0

# Visualization
plotly>=5.20

# Backend
fastapi
flask>=3.0
```

**Note:** Regenerate this file with `pip freeze > requirements.txt` from a clean virtual environment after a full successful run, and test it in a brand-new empty venv before final submission. Install time is longer due to `torch`/`transformers`/`sentence-transformers`; expect several GB of disk usage for model weights. OS-level dependencies: none beyond a standard Python/pip toolchain (poppler is bundled via `pdfplumber`/`PyMuPDF`).

---

## Demo Video

*Place the demo at `docs/demo/demo.mp4` (silent, 2–5 minutes, under 25 MB, 720p) and link it here — or link to an unlisted YouTube/Drive video if compression isn't enough:*

`[Watch the demo](docs/demo/demo.mp4)`

The demo should show, in order: login → OTP forgot-password flow → a core agent → the AI Copilot → the Admin Dashboard. Record with a fresh/dummy account — never demo using real personal Gmail credentials or a real OTP inbox on screen.

---

## Known Limitations & Future Scope

**Known Limitations:**
- Uses synthetic/seeded data (`seed_data.py`) rather than live production freight data.
- Single-tenant design — no multi-organization data isolation.
- SQLite is used for storage instead of a production-grade database (e.g. PostgreSQL).
- LLM inference runs as a local FastAPI microservice, which requires a GPU-backed environment (e.g. Colab) for practical response times.

**Future Scope:**
- Migrate from SQLite to PostgreSQL/MySQL for multi-user production deployment.
- Integrate live third-party freight-rate and AIS vessel-tracking APIs.
- Add multi-tenant organization support with per-tenant RBAC.
- Fine-tune the LLM on domain-specific maritime freight documents for improved grounding.

---

## Acknowledgements

This project was built as part of the **Infosys Springboard Internship — Batch 1**. Sincere thanks to our mentor, **Mohammed Sipli**, for guidance throughout the project.
