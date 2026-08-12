# 🏛️ Infosys Agentic AI Portal

## Milestone 2 — Full-Stack AI/ML Integration & Advanced Security Engine

A secure, professional-grade enterprise logistics platform built with **Streamlit**, unifying JWT authentication, three autonomous ML agents, an LLM Copilot, and a fully functional Admin Dashboard.

![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Python](https://img.shields.io/badge/Python-3.12-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-App-ff4b4b)
![LLM](https://img.shields.io/badge/LLM-Qwen2.5--3B--Instruct-purple)
![GPU](https://img.shields.io/badge/GPU-T4-yellow)

---

## 📌 Project Overview

Milestone 1 delivered the **User Authentication module** — JWT session handling, a Streamlit UI, SQLite-backed credentials, and Gmail-based OTP verification.

Milestone 2 unifies that security gateway with the full multi-agent ML core and an LLM Copilot, and adds three hardening layers: progressive account lockout, real-time password strength verification, and a fully functional Admin Dashboard.

---

## ✅ Milestone 2 — Scope & Deliverables

| Module | Status |
|---|---|
| Progressive Account Lockout (5/15 min/permanent) | ✅ Completed |
| Dynamic Password Strength Checker (Weak/Average/Good) | ✅ Completed |
| Agent 1 — Dynamic Pricing (6 algorithms compared) | ✅ Completed |
| Agent 2 — Route Delay Classifier (6 algorithms compared) | ✅ Completed |
| Agent 3 — Carrier Compliance Sentinel (6 algorithms compared) | ✅ Completed |
| LLM Copilot (Qwen2.5-3B-Instruct, 4-bit) | ✅ Completed |
| Admin Dashboard — Add/Delete/Unlock Users | ✅ Completed |
| Admin Dashboard — ML Model Card | ✅ Completed |
| OTP Resend Cooldown (60s/3min/5min/1hr) | ✅ Completed |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| UI / Frontend | Streamlit, `streamlit-option-menu`, Plotly |
| Authentication | bcrypt, PyJWT, SQLite |
| ML Agents | scikit-learn (RandomForest, GradientBoosting, ExtraTrees, Ridge, DecisionTree, AdaBoost, LogisticRegression, SVC), joblib |
| LLM Copilot | HuggingFace Transformers, Qwen2.5-3B-Instruct, bitsandbytes (4-bit NF4) |
| Data Sources | Kaggle API — SCMS Delivery, DataCo Supply Chain, Supply Chain Analysis, International Trade Logistics, Freight Carrier Performance, Logistics Shipment Audit |
| Hosting / Runtime | Google Colab (T4 GPU), `pyngrok` |
| Notifications | Gmail SMTP (OTP emails), in-app alert log |

---

## 🏗️ System Architecture — 4 Phases

| Phase | Module | Responsibility |
|---|---|---|
| **Phase 1** | Security Gateway | Login, Registration, Forgot Password (OTP). Stores hashed credentials & lockout state in SQLite. |
| **Phase 2** | Domain Intelligence | Unlocks Agent 1 (Pricing), Agent 2 (Route Delay), Agent 3 (Carrier Compliance) tabs. |
| **Phase 3** | Generative Advisory | LLM Copilot synthesizes agent outputs into shipping strategy + structured JSON audit. |
| **Phase 4** | System Administration | Admin Dashboard — restricted to `role = 'Admin'`. |

---

## 🌍 Localized Indian Port Coverage

| Port | Code / Location | Region |
|---|---|---|
| Mumbai | JNPT (Jawaharlal Nehru Port Trust) | West Coast, Maharashtra |
| Mundra | Mundra Port | West Coast, Gujarat |
| Chennai | Chennai Port | East Coast, Tamil Nadu |
| Cochin | Cochin Port | South Coast, Kerala |

---

## ⚙️ Setup Instructions

### 1️⃣ Colab Runtime & GPU
- **Runtime → Change runtime type → T4 GPU → Save**
- Run `!nvidia-smi` as the first cell to confirm the GPU is attached
- Qwen2.5-3B-Instruct loads with `load_in_4bit=True` for low VRAM usage on a single T4

### 2️⃣ Kaggle API Setup (Recommended)
- Log in at [kaggle.com](https://kaggle.com) → profile → **Settings → API → Create New Token**
- Downloads a `kaggle.json` with username + key
- Add both as Colab Secrets, or upload to `~/.kaggle/kaggle.json`

### 3️⃣ Colab Secrets Setup
Click the 🔑 **Secrets** icon in the Colab sidebar and add:

| Secret Name | Purpose |
|---|---|
| `JWT_SECRET_KEY` | Signs & verifies login session tokens |
| `ADMIN_EMAIL_ID` | Admin panel login (default: `infosys@ai`) |
| `ADMIN_PASSWORD` | Admin panel password (default: `admin@123`) |
| `NGROK_AUTHTOKEN` | Public HTTPS URL for the Streamlit app |
| `HF_TOKEN` | HuggingFace access for the LLM Copilot |
| `EMAIL_ID` / `EMAIL_PASSWORD` | Gmail SMTP for real OTP emails |
| `KAGGLE_USERNAME` / `KAGGLE_KEY` | Real Kaggle data instead of synthetic |

**Note:** No secrets are hard-coded — every value is read via Colab Secrets with an environment-variable fallback.

---

## ▶️ How to Run

1. Open `Infosys_AgenticAI_Milestone2.ipynb` in Google Colab
2. Complete the Setup steps above
3. **Runtime → Run all**
4. Open the printed public HTTPS URL
5. Log in with your `ADMIN_EMAIL_ID` / `ADMIN_PASSWORD` (or default `infosys@ai` / `admin@123`)
6. Run the final "Stop Application" cell to free GPU memory when done

---

## 🔒 Progressive Account Lockout

| Failed Attempts | Action | Message |
|:---:|---|---|
| 3rd | Lock 5 minutes | ⏳ Account temporarily locked for 5 minutes due to 3 failed attempts. |
| 4th | Lock 15 minutes | ⏳ Account temporarily locked for 15 minutes due to 4 failed attempts. |
| 5th | Permanent lock | ❌ Account permanently locked. Only Admin can unlock via the Dashboard. |

---

## ⏱️ OTP Resend Cooldown

| Resend Attempt | Cooldown | Message |
|---|---|---|
| 1st | 60 sec | ⏳ Please wait 60 seconds before requesting another OTP. |
| 2nd | 3 min | ⏳ Please wait 3 minutes before requesting another OTP. |
| 3rd | 5 min | ⏳ Please wait 5 minutes before requesting another OTP. |
| 4th+ | 1 hour | ⚠️ Too many OTP requests. Please wait 1 hour. |

---

## 🔑 Password Strength Policy

| Length | Badge | Behavior |
|---|:---:|---|
| < 5 characters | 🔴 Weak | Blocked — cannot register |
| 5–9 characters | 🟡 Average | Allowed |
| 10+ characters | 🟢 Good | Allowed — bcrypt hashed |

---

## 🤖 ML Model Training — 5+ Algorithms Per Agent

| Agent | Metric | Algorithms |
|---|---|---|
| **Agent 1: Dynamic Pricing** | R² ≥ 0.90 | RandomForest, GradientBoosting, ExtraTrees, Ridge, DecisionTree, AdaBoost |
| **Agent 2: Route Delay** | ROC-AUC | RandomForest, GradientBoosting, LogisticRegression, SVC (RBF), ExtraTrees, AdaBoost |
| **Agent 3: Carrier Compliance** | ROC-AUC | GradientBoosting, RandomForest, ExtraTrees, LogisticRegression, DecisionTree, AdaBoost |

---

## 🛡️ Admin Dashboard — User Lifecycle Management

- **👥 User Management** — Add User, Delete User, Unlock Account
- **📈 ML Model Card** — Champion model + metrics for all 3 agents
- **🔔 System & Alerts** — GPU/system health, LLM activity monitor, live alert log

---

## 💬 AI Copilot

Ask anything, e.g. *"Explain in 2 sentences why port congestion increases freight risk."*
- Real Qwen2.5-3B (4-bit) response when GPU is loaded
- Rule-based fallback otherwise — expected behavior, not a bug
- Synthesizes Agent 1–3 outputs into a structured JSON audit action

---

## 📸 ScreenShots :

### 🏠 Home Page
<img width="1920" height="1080" alt="Screenshot (79)" src="https://github.com/user-attachments/assets/8286eb6d-ef43-476f-a7ea-bf54090f1500" />





### 💬 AI Copilot

<img width="1920" height="1080" alt="Screenshot (78)" src="https://github.com/user-attachments/assets/d68af611-4618-4103-87bb-824ba3f6a962" />





### 💰 ML Pricing Calculator

<img width="1920" height="1080" alt="Screenshot (80)" src="https://github.com/user-attachments/assets/061b186f-a3e4-4c0e-8bf1-1194ecf16755" />



### 📈 Admin — ML Model Card

<img width="1920" height="1080" alt="Screenshot (81)" src="https://github.com/user-attachments/assets/a1e8c438-4642-4cf4-a0e1-962016c211d8" />




### 👥 Admin — Add / Delete / Unlock User
<img width="1920" height="1080" alt="Screenshot (82)" src="https://github.com/user-attachments/assets/4c999e46-340a-4f48-84d7-890500ef3bb5" />




### ⏱️ OTP Resend Cooldown

<img width="1920" height="1080" alt="Screenshot (83)" src="https://github.com/user-attachments/assets/232566aa-448c-4981-b55a-c7b5d75cc0ed" />




### 🔒 Account Lockout Message

<img width="1920" height="1080" alt="Screenshot (84)" src="https://github.com/user-attachments/assets/996f1298-e341-48db-9627-7e8ce1484391" />


---

## ✅ Final Checklist

- [x] `Milestone2` folder created inside existing Infosys Repository
- [x] README.md added — explains what Milestone 2 adds, includes port coverage table
- [x] Colab runtime set to T4 GPU
- [x] Kaggle API configured (or synthetic fallback confirmed working)
- [x] All secrets set up via Colab Secrets
- [x] Login / Signup / Forgot Password (OTP) gate all agent & analytics tabs
- [x] Progressive lockout (5/15/permanent) implemented and tested
- [x] OTP resend cooldown implemented and tested
- [x] Password strength checker implemented on registration and reset
- [x] Agent 1 (Pricing): 5+ algorithms compared, R² ≥ 0.90 confirmed
- [x] Agent 2 (Route Delay): 5+ algorithms compared, ROC-AUC optimized
- [x] Agent 3 (Carrier Compliance): 5+ algorithms compared, ROC-AUC optimized
- [x] AI Copilot returns real Qwen2.5-3B response + structured JSON audit action
- [x] Admin Dashboard: Add/Delete/Unlock all functional
- [x] Admin Panel → ML Model Card shows metrics for all 3 agents
- [x] Screenshots captured and linked in README.md
- [x] All personal secrets removed before upload
- [x] Notebook restarted, re-run top to bottom, outputs cleared, uploaded as `Infosys_AgenticAI_Milestone2.ipynb`

## 📧 Support

For issues or questions regarding this milestone, please reach out to the project mentor or raise an issue in the internal tracking system.
