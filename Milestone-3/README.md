# 🏛️ FreightQuote AI — Enterprise Multi-Agent Logistics Intelligence Platform

**Infosys Springboard Internship Project — Milestone 3**

FreightQuote AI is an enterprise-grade logistics intelligence platform that combines secure authentication, multi-agent machine learning, an LLM-powered reasoning engine, and a Retrieval-Augmented Generation (RAG) knowledge center — all delivered through a unified Streamlit dashboard styled in a Navy & Gold professional theme.

---

## Table of Contents

- [Overview](#overview)
- [Milestone Architecture](#milestone-architecture)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Setup & Installation](#setup--installation)
- [Environment Secrets](#environment-secrets)
- [Running the Project](#running-the-project)
- [RAG Knowledge Center](#rag-knowledge-center)
- [Machine Learning Agents](#machine-learning-agents)
- [Evaluation & Testing](#evaluation--testing)
- [Contributors](#contributors)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

---

## Overview

FreightQuote AI helps global logistics teams generate freight quotes, assess delay risk, audit carrier compliance, and query internal logistics documentation — all backed by machine learning models and a locally-hosted LLM (Qwen-2.5-3B-Instruct, 4-bit quantized).

The project was built incrementally across three milestones and is submitted here as a combined, production-ready deliverable.

---

## Milestone Architecture

**Milestone 1 — Authentication Layer**

- Streamlit login/register portal
- JWT session tokens + bcrypt password hashing
- Email OTP password recovery
- Progressive lockout & password-strength enforcement

**Milestone 2 — Multi-Agent Logistics Intelligence Platform**

- Agent 1: Pricing Engine (freight cost prediction)
- Agent 2: Route Optimization & Marine Weather Risk
- Agent 3: Carrier Audit & Tariff Compliance
- Qwen-2.5-3B-Instruct (4-bit) — multi-agent debate & synthesis
- Admin Dashboard (user management, ML model card, system health)

**Milestone 3 — RAG Knowledge Center**

- 100+ ingested logistics SOP, compliance, and pricing PDF documents
- FAISS vector index built on `all-MiniLM-L6-v2` embeddings
- Qwen-2.5-3B-Instruct answering engine with a CPU rule-based fallback
- Automated 32-query evaluation suite
- Streamlit "Knowledge Center" dashboard, styled to match the Navy & Gold theme

All three milestones share the same design language, Google Drive–backed persistent storage (`STORAGE_DIR`), and Colab Secrets–based credential management.

---

## Key Features

| Module | Description |
|---|---|
| Authentication | JWT + bcrypt login, registration, OTP email recovery, progressive lockout |
| Agent 1 — Pricing | Predicts freight cost using route, weight, mode, and port congestion features |
| Agent 2 — Route & Weather Risk | Assesses delay risk using marine weather, port congestion, and route data |
| Agent 3 — Carrier Audit | Flags carriers failing tariff compliance and safety thresholds |
| Multi-Agent LLM Reasoning | Qwen-2.5-3B-Instruct orchestrates agent debate and synthesis for final recommendations |
| Admin Dashboard | User management, ML model metrics (R² / RMSE / ROC-AUC), live system alerts |
| RAG Knowledge Center | Semantic search and QA over logistics SOPs, customs, carrier, and pricing policy documents |
| Automated Evaluation | 32-question test suite validating RAG retrieval accuracy and latency |

---

## Tech Stack

**Frontend / UI**
Streamlit, streamlit-option-menu, Plotly

**Backend / Auth**
SQLite, PyJWT, bcrypt, smtplib (OTP email)

**Machine Learning**
scikit-learn (RandomForest, GradientBoosting, ExtraTrees, Ridge, AdaBoost, SVM, Logistic/Decision Trees), joblib for model persistence

**LLM & RAG**
`Qwen/Qwen2.5-3B-Instruct` (4-bit NF4, via `transformers` + `bitsandbytes`), `sentence-transformers` (`all-MiniLM-L6-v2`) for embeddings, FAISS for vector similarity search, `langchain-text-splitters` for recursive chunking, `reportlab` / `pypdf` for knowledge base PDF generation and parsing

**Infrastructure**
Google Colab (T4 GPU runtime), Google Drive (persistent model and index storage), ngrok (public tunnel for Streamlit apps)

---

## Repository Structure

Infosys_FreightQuote_AI-/
├── Milestone1/
├── Milestone2/
├── Milestone3/
│ ├── FreightQuote_AI_Milestone1_2_Combined.ipynb
│ ├── FreightQuote_AI_RAG.ipynb
│ └── README.md
├── LICENSE
└── README.md


Application modules (`app.py`, `auth.py`, `db.py`, `agents*.py`, `rag_pipeline.py`, `evaluator.py`, etc.) are generated at runtime inside the notebooks via `%%writefile` cells, so the source of truth for all code lives in the `.ipynb` files.

---

## Setup & Installation

**Prerequisites**

- Google account with access to Google Colab
- Google Drive (for persistent storage of models, database, and FAISS index)
- ngrok account (free tier is sufficient) for public app URLs
- Optional: Hugging Face account/token if using gated models

**Steps**

1. Open both notebooks in Google Colab.
2. Set Runtime → Change runtime type → GPU (T4).
3. Add the required secrets in Colab's Secrets panel (see below).
4. Run all cells top to bottom in each notebook.

---

## Environment Secrets

Configure these under Colab Secrets (key icon in the left sidebar):

| Secret Name | Required | Purpose |
|---|---|---|
| `NGROK_AUTHTOKEN` | Yes | Publishes the Streamlit app via a public URL |
| `HF_TOKEN` | Optional | Hugging Face token (only needed for gated models) |
| `KAGGLE_USERNAME` / `KAGGLE_KEY` | Optional | Downloading supplementary training datasets |
| `EMAIL_ID` / `EMAIL_PASSWORD` | Optional | Sending OTP emails for password recovery |
| `JWT_SECRET_KEY` | Optional | Signs JWT session tokens (falls back to a dev default) |
| `ADMIN_EMAIL_ID` / `ADMIN_PASSWORD` | Optional | Seeds the default admin account |

No credentials are hardcoded in the source — all secrets are read via a `_get_secret()` helper that checks Colab Secrets first, then environment variables.

---

## Running the Project

**1. Combined Milestone 1 & 2 Notebook**

1. Installs dependencies
2. Mounts Google Drive → `STORAGE_DIR`
3. Loads Qwen-2.5-3B-Instruct (4-bit)
4. Writes all application modules (`auth.py`, `db.py`, `agents*.py`, `admin_dash.py`, `app.py`)
5. Initializes and seeds the SQLite database
6. Trains all ML agents (six algorithms compared per agent, best model persisted)
7. Launches the Streamlit dashboard via ngrok

**2. RAG Notebook**

1. Installs RAG dependencies
2. Mounts the same `STORAGE_DIR` for shared persistence
3. Writes `rag_pipeline.py`, `evaluator.py`, `app.py`
4. Generates 100+ logistics SOP PDFs, plus two intentionally corrupted files to test error handling
5. Chunks, embeds, and indexes documents into FAISS
6. Runs the automated 32-query evaluation suite
7. Launches the RAG Knowledge Center dashboard via ngrok

Both notebooks must remain active and running on a T4 GPU runtime during live evaluation and demo sessions.

---

## RAG Knowledge Center

The RAG module ingests logistics domain documents across five categories:

- Customs Compliance — HTS codes, manifests, bonded warehouses, HAZMAT rules
- Route Optimization — port congestion, weather-based routing, dwell times
- Carrier Safety & Compliance — tariff audits, insurance minimums, tier ratings
- Pricing & Surcharges — fuel indexing, peak season rates, accessorial fees
- Cargo Insurance — liability limits, claims windows, reefer compliance

**Pipeline:** PDF ingestion → recursive chunking (chunk size 600, overlap 60) → `all-MiniLM-L6-v2` embeddings → FAISS `IndexFlatIP` (cosine similarity) → persisted to Google Drive → Qwen-2.5-3B answering with citation of source document and page, or a CPU rule-based fallback when no GPU is available.

---

## Machine Learning Agents

| Agent | Task | Algorithms Compared | Metric |
|---|---|---|---|
| Agent 1 — Pricing | Freight cost regression | RandomForest, GradientBoosting, ExtraTrees, Ridge, DecisionTree, AdaBoost | R² |
| Agent 2 — Delay Risk | Delay probability classification | Calibrated RF / GB / LR / SVM / ExtraTrees / AdaBoost | ROC-AUC |
| Agent 3 — Carrier Audit | Compliance pass/fail classification | Calibrated GB / RF / ExtraTrees / LR / DecisionTree / AdaBoost | ROC-AUC |

The best-performing model per agent is automatically selected and persisted to `models/` on Google Drive; all training runs are logged to the `ml_models` table and visible in the Admin Dashboard's ML Model Card tab.

---

## Evaluation & Testing

- **RAG Evaluation Suite:** 32 hand-crafted logistics questions spanning all five knowledge categories, graded on retrieval similarity score (above a 0.25 threshold) and answer relevance. Results and per-query latency are compiled into a report inside the notebook.
- **Live Testing Readiness:** Both notebooks are kept running on Colab T4 GPU with active ngrok tunnels so the RAG pipeline and multi-agent dashboard can be demonstrated live with unscripted queries.

---

## Contributors

| Name | Responsibility |
|---|---|
| **Manuru Deepika** | Tested and verified the RAG pipeline, validated Hugging Face model integration, and configured ngrok deployment. |
| Smita Barada | Developed the Retrieval-Augmented Generation (RAG) pipeline notebook, including knowledge base generation, retrieval, and semantic search workflow. |
| Sriharsha Thorupunuri | Prepared the complete project documentation, created and maintained the README, and managed the GitHub repository workflow. |
| Syed Saleem Malik | Designed and implemented the Streamlit frontend UI across the application's authentication, dashboard, and admin panels. |
| Sravya Nanda | Tested and validated the OTP verification flow and dashboard functionality across user and admin views. |

---

## Acknowledgements

- Infosys Springboard for providing the internship opportunity.
- Our mentors for their continuous guidance and technical support.
- Saveetha School of Engineering for encouraging innovation and project-based learning.
- Our teammates for their collaboration, dedication, and valuable contributions throughout the project.

---

## Contact

For project-related queries or collaboration, please contact the project team through the GitHub repository or the Infosys Springboard platform.




Claude is AI and can make mistakes. Please double-itory or the Infosys Springboard platform.

