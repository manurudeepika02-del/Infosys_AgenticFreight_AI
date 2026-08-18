# ⚡ Maritime Freight Portal — FreightQuote AI

### End-to-End AI-Powered Maritime Freight Intelligence Platform (Milestone 4)

---

## 📌 Overview

**FreightQuote AI** is an end-to-end, AI-powered maritime freight intelligence platform. It helps logistics teams get instant, grounded answers about shipments, freight pricing, carrier performance, customs, weather risk, and documentation.

The project is built across **three integrated notebooks**, each responsible for a distinct layer of the system:

| Notebook | Purpose |
|----------|---------|
| `Maritime_Freight_Portal_Milestone4.ipynb` | The main Streamlit application — UI, authentication, all 9 AI agents, admin dashboard, and deployment |
| `Copy_of_FreightQuote_Data_Pipeline_ML_Trainer.ipynb` | Data pipeline & AutoML trainer — downloads Kaggle datasets and trains ML models for each agent |
| `FreightQuote_RAG_Builder__3_.ipynb` | Knowledge base builder — crawls documents/PDFs and builds the FAISS + BM25 retrieval indexes powering the RAG-based AI Copilot |

Together, these three notebooks form a complete pipeline: **data & model training → knowledge base indexing → deployed intelligent portal.**

---

## 🏗️ System Architecture

```
                ┌─────────────────────────────────────┐
                │   1. Data Pipeline & ML Trainer      │
                │   (Kaggle datasets → trained models) │
                └───────────────────┬───────────────────┘
                                    │  ML models (joblib)
                                    ▼
┌───────────────────────┐   ┌─────────────────────────────┐
│  2. RAG Builder        │   │  3. Maritime Freight Portal  │
│  (PDF/Web crawl →      │──▶│  (Streamlit app: UI, agents, │
│  FAISS + BM25 index)   │   │  auth, admin, LLM backend)   │
└───────────────────────┘   └─────────────────────────────┘
```

---

## ✨ Key Features

### 🔐 Authentication & Access Control
- Email/username login with bcrypt password hashing and JWT-based sessions
- Gmail OTP-based password reset with built-in SMTP diagnostics
- Role-Based Access Control (Admin, Operations Manager, Freight Broker, and more)

### 🤖 AI Copilot (RAG-powered)
- Grounded Q&A over live shipment, port, and pricing data
- Intent classification with automatic routing to the correct data agent
- Backed by a hybrid **FAISS + BM25** retrieval engine built in the RAG Builder notebook

### 🚢 Freight & Port Intelligence Agents
| Agent | Capability |
|-------|-----------|
| Agent 1 | Route Intelligence — interactive port maps and route analysis |
| Agent 2 | Dynamic Freight Pricing Engine |
| Agent 3 | Carrier Performance & Capacity |
| Agent 4 | Weather-Aware Freight Risk |
| Agent 5 | Dynamic Margin Predictor |
| Agent 6 | Customs, Tariffs & Compliance |
| Agent 7 | Digital Bill of Lading & Document Management |
| Agent 8 | Real-Time Incident Alerts & Multilingual Translation Studio |
| Agent 9 | PDF SOP & Freight Document RAG Studio |

### 🧠 Data Pipeline & AutoML Trainer
- Automatically downloads and merges Kaggle datasets (logistics, pricing, weather, customs, sentiment, insurance)
- Auto-detects the correct target column and task type (classification/regression) per agent
- Trains and benchmarks a 10-model competition (Random Forest, Gradient Boosting, XGBoost, AdaBoost, Extra Trees, etc.) for each of the 7 core prediction agents:
  1. Route Intelligence — arrival probability
  2. Dynamic Pricing — freight cost
  3. Weather Disruptions — delay prediction
  4. Customs Compliance — clearance prediction
  5. Margin Optimization — margin prediction
  6. Client Support — sentiment analysis
  7. Cargo Insurance — claim prediction
- Saves best-performing models to Google Drive for use by the portal

### 📚 Knowledge Base / RAG Builder
- Crawls seed websites to automatically discover and download hosted PDFs (no hardcoded document list)
- Extracts text from PDFs/HTML and chunks it for retrieval
- Builds a **synthetic freight knowledge base** covering Incoterms, customs, cargo insurance, and shipping SOPs
- Generates FAISS vector index + BM25 keyword index for hybrid semantic + keyword search
- Powers the AI Copilot and Agent 9 (PDF SOP & Document RAG Studio) with grounded, citation-ready answers

### 📊 Analytics & Monitoring
- Anomaly & risk scanner (Isolation Forest) across shipments, ports, and quotes
- Knowledge graph visualization of ports, carriers, and documents
- Global Ocean Freight Digital Twin with Monte Carlo stress testing
- Admin Dashboard — system health, user lifecycle management, LLM activity monitor, live alert log

### 🗄️ Data & Infrastructure
- SQLite database with automated seeding (50+ global ports)
- Kaggle data ingestion pipeline with schema validation
- FastAPI microservice serving a local LLM with streaming responses
- Deployed via Cloudflare Tunnel for public access from Google Colab

---

## 🛠️ Tech Stack

`Streamlit` `FastAPI` `SQLite` `FAISS` `BM25 (rank-bm25)` `Sentence Transformers` `Hugging Face Transformers` `PyTorch` `Plotly` `Folium` `bcrypt` `PyJWT` `NLLB-200 (translation)` `scikit-learn` `XGBoost` `LangChain` `BeautifulSoup / PyMuPDF (web & PDF crawling)`

---

## 🖼️ Screenshots

> Add screenshots under each subheading below to visually document the application.

### 1. Login & Authentication
- Login page (Classic Navy & Gold theme)
- OTP email verification screen
- Registration form with security question

### 2. Admin Dashboard
- System health cards (GPU, uptime, LLM status)
- User lifecycle management panel
- Live alert log

### 3. AI Copilot
- Chat interface with a sample grounded query and response

### 4. Route Intelligence (Agent 1)
- Interactive port map with route overlay

### 5. Freight Pricing & Margin (Agents 2 & 5)
- Pricing engine dashboard with charts
- Margin predictor output

### 6. Carrier Performance (Agent 3)
- Carrier capacity/performance charts

### 7. Weather-Aware Risk (Agent 4)
- Weather risk map/dashboard

### 8. Customs & Compliance (Agent 6)
- Tariff/customs lookup screen

### 9. Document Management (Agent 7)
- Digital Bill of Lading view

### 10. Alerts & Translation Studio (Agent 8)
- Real-time incident alert feed
- Multilingual translation interface (showing 2–3 languages)

### 11. PDF RAG Studio (Agent 9)
- Document upload and Q&A result

### 12. Anomaly Scanner
- Isolation Forest anomaly results table/chart

### 13. Knowledge Graph
- Full knowledge graph visualization

### 14. Digital Twin
- Global freight network simulation view

### 15. My Profile
- User self-service profile page

### 16. Data Pipeline & ML Trainer (Notebook)
- Kaggle dataset download/merge logs
- 10-model competition results/leaderboard per agent

### 17. RAG Builder (Notebook)
- PDF/web crawler discovery log
- FAISS + BM25 index build confirmation

---

## 👥 Team Contributions

| Name | Contribution |
|------|--------------|
| **Alakshya Nilesh Salvi** | Authentication module (login, registration, RBAC) and Gmail OTP integration |
| **Simran Kapoor** | AI Copilot, LLM engine, and RAG pipeline (FAISS/BM25 hybrid retrieval, RAG Builder notebook) |
| **Nadendla Padhvika** | Freight & pricing agents (Route Intelligence, Dynamic Pricing, Margin Predictor) and analytics dashboards |
| **S Uday Gowda** | Admin Dashboard, database design/seeding, deployment (FastAPI backend, Cloudflare Tunnel), and Data Pipeline/ML Trainer notebook |
| **Manuru Deepika** | UI theme design, Multilingual Translation Studio, and documentation |

---

## 🔑 Default Admin Login

```
Email: admin@infosys.com
Password: admin123
```

---

## ⚙️ Setup Notes

- Requires `EMAIL_ID` and a Google App Password (as Colab Secrets) for real Gmail OTP delivery
- Run notebooks in order for a fresh setup:
  1. `Copy_of_FreightQuote_Data_Pipeline_ML_Trainer.ipynb` — trains and saves ML models to Google Drive
  2. `FreightQuote_RAG_Builder__3_.ipynb` — builds the FAISS/BM25 knowledge base
  3. `Maritime_Freight_Portal_Milestone4.ipynb` — launches the full Streamlit application
- Runs on Google Colab with GPU acceleration recommended
- Public access enabled via Cloudflare Tunnel (`cloudflared`)

---

<p align="center"><i>Built as part of the Infosys Springboard Internship Program.</i></p>
