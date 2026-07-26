# FreightQuote AI Platform — Milestone 2
### Full-Stack AI/ML Integration & Advanced Security Engine

![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Python](https://img.shields.io/badge/Python-3.12-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-App-ff4b4b)
![LLM](https://img.shields.io/badge/LLM-Qwen2.5--3B--Instruct-purple)
![GPU](https://img.shields.io/badge/GPU-T4-yellow)
![License](https://img.shields.io/badge/Project-Infosys%20Springboard-orange)

---

## 1. What Milestone 2 Adds Over Milestone 1

Milestone 1 delivered the **User Authentication module** — JWT session handling, a Streamlit UI, SQLite-backed credentials, and Gmail-based OTP verification for password resets.

Milestone 2 builds on top of that security gateway and unifies it with the full multi-agent ML core and an LLM Copilot. Specifically, this milestone adds:

- **Progressive account lockout** — accounts are temporarily (and eventually permanently) locked after repeated failed logins, with tiered lockout durations.
- **Dynamic, real-time password strength verification** — a live Weak / Average / Good badge shown during registration and password reset.
- **Three autonomous ML agents** (Dynamic Pricing, Route Delay Classifier, Carrier Compliance Sentinel), each trained on 2 assigned Kaggle datasets and comparing **5+ algorithms** before a champion model is selected and saved.
- **An LLM Copilot** (Qwen2.5-3B-Instruct, 4-bit quantized) that synthesizes all three agents' numerical outputs into an executive shipping strategy and a structured JSON audit action, with an automatic rule-based fallback while the model is warming up or unavailable.
- **A fully functional Admin Dashboard** — Add / Delete / Unlock user lifecycle controls, an ML Model Card view of every agent's training metrics, and live system/alert monitoring — restricted exclusively to accounts with `role = 'Admin'`.

## 2. Tech Stack

| Layer | Technology |
|---|---|
| UI / Frontend | Streamlit, `streamlit-option-menu`, Plotly |
| Authentication | bcrypt (password hashing), PyJWT (session tokens), SQLite |
| ML Agents | scikit-learn (RandomForest, GradientBoosting, ExtraTrees, Ridge, DecisionTree, AdaBoost, LogisticRegression, SVC), joblib |
| LLM Copilot | HuggingFace Transformers, Qwen2.5-3B-Instruct, bitsandbytes (4-bit NF4 quantization) |
| Data Sources | Kaggle API (`kagglehub`/`kaggle`) — SCMS Delivery, DataCo Supply Chain, Supply Chain Analysis, International Trade Logistics, Freight Carrier Performance, Logistics Shipment Audit datasets |
| Hosting / Runtime | Google Colab (T4 GPU), `pyngrok` for public HTTPS tunneling |
| Notifications | Gmail SMTP (OTP emails), in-app alert log |

## 3. System Architecture — 4 Phases

| Phase | Module / Component | Responsibility & Workflow |
|---|---|---|
| **Phase 1: Security Gateway** | Authentication & JWT | Enforces Login, Registration, and Forgot Password (Gmail OTP) before unlocking the UI. Stores hashed credentials and progressive lockout state in SQLite (`users` table). |
| **Phase 2: Domain Intelligence** | 3 Autonomous Agents | Once authenticated, unlocks **Agent 1: Dynamic Pricing**, **Agent 2: Route Delay Classifier**, and **Agent 3: Carrier Compliance Sentinel** tabs. |
| **Phase 3: Generative Advisory** | LLM Copilot & JSON | Integrates HuggingFace LLM orchestration (`llm_engine_freight.py`) to synthesize the 3 agents' numerical outputs into executive shipping strategies and structured JSON audit actions. |
| **Phase 4: System Administration** | Admin Dashboard | Dedicated administrative controls (`admin_dash.py`) restricted exclusively to users authenticated with `role = 'Admin'`. |

## 4. Localized Indian Port Coverage

| Port | Code / Location | Region |
|---|---|---|
| Mumbai | JNPT (Jawaharlal Nehru Port Trust) | West Coast, Maharashtra |
| Mundra | Mundra Port | West Coast, Gujarat |
| Chennai | Chennai Port | East Coast, Tamil Nadu |
| Cochin | Cochin Port | South Coast, Kerala |

These ports are used throughout Agent 2 (Route/Weather) and the merged dataset seeding as representative Indian shipment origins/destinations.

## 5. Setup Instructions

### 5.1 Colab Runtime & GPU

1. **Runtime → Change runtime type → T4 GPU → Save.**
2. Run `!nvidia-smi` as the first code cell and confirm the GPU is attached before continuing.
3. The Qwen2.5-3B-Instruct model is loaded with `load_in_4bit=True` (bitsandbytes NF4) to keep VRAM usage low and load time fast on a single T4.

### 5.2 Kaggle API Setup (Recommended)

Real logistics data (SCMS Delivery + DataCo Supply Chain, and the other 4 datasets) is used when Kaggle credentials are available; the notebook falls back to synthetic data automatically if not.

1. Log in at [kaggle.com](https://kaggle.com) → profile picture → **Settings → API → Create New Token**.
2. This downloads a `kaggle.json` file containing a username and key.
3. Add both as Colab Secrets (see below), or upload the file to `~/.kaggle/kaggle.json`.

### 5.3 Colab Secrets Setup

Click the **key icon (🔑 Secrets)** in the left sidebar of Colab, add each secret below, and toggle **Notebook access ON** for each.

| Secret Name | How to Get It | Used For |
|---|---|---|
| `JWT_SECRET_KEY` | Any long random string you make up — never transmitted, only signs tokens locally | Signs & verifies login session tokens |
| `ADMIN_EMAIL_ID` | Any email you choose — becomes the Admin Panel login (seed: `infosys@ai` as fallback default) | Bootstraps the admin account on first run |
| `ADMIN_PASSWORD` | Any password meeting the strength rule (8+ chars, upper, lower, number, symbol) | Bootstraps the admin account on first run |
| `NGROK_AUTHTOKEN` | Free account at [ngrok.com](https://ngrok.com) → dashboard → copy Authtoken | Gives the Streamlit app a public HTTPS URL |
| `HF_TOKEN` | HuggingFace account → Settings → Access Tokens | Authenticates HuggingFace LLM Copilot inference (Qwen2.5-3B 4-bit) |
| `EMAIL_ID` | The Gmail address OTP/alert emails are sent from | Sender address for real OTP emails (optional — console fallback works without it) |
| `EMAIL_PASSWORD` | Gmail → 2-Step Verification → App Passwords → create a 16-character app password | Authenticates the Gmail SMTP sender for OTP emails |
| `KAGGLE_USERNAME` / `KAGGLE_KEY` | From the `kaggle.json` downloaded in 5.2 | Optional — trains models on real Kaggle data instead of synthetic |

**Note:** Secrets are never hard-coded anywhere in this notebook — every value is read via Colab Secrets with an environment-variable fallback.

## 6. How to Run

1. Open `FreightQuote_AI_Milestone2.ipynb` in Google Colab.
2. Complete Sections 5.1–5.3 above (GPU runtime, Kaggle API, Colab Secrets).
3. Run all cells top to bottom (**Runtime → Run all**), or step through them in order:
   - Install dependencies
   - Configure secrets & mount Google Drive
   - Verify GPU & load the LLM
   - Write all application modules (`auth.py`, `db.py`, `ui_theme.py`, `admin_dash.py`, `train_ml_freight.py`, `llm_engine_freight.py`, etc.)
   - Initialize the database & seed sample data
   - Train the 3 ML agents (5+ algorithms each)
   - Launch the Streamlit app via ngrok
4. Open the printed public HTTPS URL.
5. Log in with your `ADMIN_EMAIL_ID` / `ADMIN_PASSWORD` (or the default `infosys@ai` / `admin@123` if those secrets weren't set) to access the Admin Dashboard.
6. To stop the app and free GPU memory, run the final "Stop Application" cell.

## 7. Progressive Account Lockout

| Failed Attempts | Severity | Action | User-Facing Message |
|:---:|:---:|---|---|
| 3rd consecutive | ![Temp](https://img.shields.io/badge/Temp--Lock-5min-yellow) | `lock_until = now() + 5 min` | ⏳ Account temporarily locked for 5 minutes due to 3 failed attempts. |
| 4th consecutive | ![Temp](https://img.shields.io/badge/Temp--Lock-15min-orange) | `lock_until = now() + 15 min` | ⏳ Account temporarily locked for 15 minutes due to 4 failed attempts. |
| 5th consecutive | ![Locked](https://img.shields.io/badge/Locked-Permanent-red) | `account_status = 'locked'` | ❌ Account permanently locked due to 5 failed attempts. Only the System Administrator can unlock this account via the Admin Dashboard. |

On a successful login where `now() >= lock_until`, the system automatically resets `failed_attempts = 0` and `lock_until = NULL`.

## 8. OTP Resend Cooldown

| Resend Attempt | Cooldown | Notification |
|---|---|---|
| 1st resend | 60 seconds | ⏳ Please wait 60 seconds before requesting another OTP. |
| 2nd resend | 180 seconds (3 min) | ⏳ Please wait 3 minutes before requesting another OTP. |
| 3rd resend | 300 seconds (5 min) | ⏳ Please wait 5 minutes before requesting another OTP. |
| 4th+ resend | 3,600 seconds (1 hour) | ⚠️ Too many OTP requests. Please wait 1 hour before trying again. |

## 9. Password Strength Policy & Real-Time Checker

Applies during registration and password reset. The checker updates live as the user types, and blocks submission below the minimum threshold.

| Length | Strength Badge | Submission Behavior | System Message |
|---|:---:|---|---|
| **< 5 characters** | ![Weak](https://img.shields.io/badge/Weak-red) | ❌ **BLOCKED** — insertion into the database is prevented | "Password too weak (minimum 5 characters required)." |
| **5–9 characters** | ![Average](https://img.shields.io/badge/Average-yellow) | ✅ Allowed | "Average strength (10+ characters recommended for enterprise security)." |
| **10+ characters** | ![Good](https://img.shields.io/badge/Good-brightgreen) | ✅ Allowed — proceeds to bcrypt hashing | "Good password strength — proceed with bcrypt hashing." |

**Implementation notes:**
- Enforced identically on **Register Account** (Tab 2) and **Reset Password** (Tab 3) screens.
- The badge and message render live below the password field on every keystroke via `render_password_strength()` in `auth.py`.
- Weak passwords never reach the database — the `st.warning()` block halts submission before any `INSERT`/`UPDATE` executes.
- All accepted passwords (Average or Good) are hashed with **bcrypt** before storage; plaintext passwords are never persisted.

## 10. ML Model Training — 5+ Algorithms Per Agent

| Agent | Metric | Algorithms Compared |
|---|---|---|
| **Agent 1: Dynamic Pricing** (Regression) | R² ≥ 0.90 | RandomForestRegressor, GradientBoostingRegressor, ExtraTreesRegressor, Ridge, DecisionTreeRegressor, AdaBoostRegressor |
| **Agent 2: Route Delay Classifier** (Classification) | ROC-AUC | RandomForestClassifier, GradientBoostingClassifier, LogisticRegression, SVC (RBF), ExtraTreesClassifier, AdaBoostClassifier |
| **Agent 3: Carrier Compliance Sentinel** (Classification) | ROC-AUC | GradientBoostingClassifier, RandomForestClassifier, ExtraTreesClassifier, LogisticRegression, DecisionTreeClassifier, AdaBoostClassifier |

Each agent trains on its 2 assigned Kaggle datasets, logs every algorithm's metrics to the `ml_models` table, and saves the best-performing model to disk via `joblib`.

## 11. Admin Dashboard — User Lifecycle Management

Accessible only to accounts with `role = 'Admin'`. Organized into three tabs:

- **👥 User Management** — Add User (custom username, email, password, role), Delete User, and Unlock Account (for any user with `account_status='locked'` or `failed_attempts >= 3`).
- **📈 ML Model Card** — Champion model and metrics (R²/RMSE for Pricing, ROC-AUC for Route Delay & Carrier Audit) for all 3 agents, pulled live from the `ml_models` table.
- **🔔 System & Alerts** — GPU/system health, LLM Copilot activity monitor, and the live alert log.

## 12. AI Copilot

Once the app launches, open the **AI Copilot** page and submit a prompt (e.g. *"Explain in 2 sentences why port congestion increases freight risk"*):

- If a GPU is attached and Qwen2.5-3B (4-bit) has loaded successfully, you get a real model response.
- Otherwise, a rule-based fallback answers immediately instead of the UI waiting on the model to load — expected behavior, not a bug.
- The Copilot also synthesizes Agent 1–3 outputs into a structured JSON audit action (Phase 3).

## 13. Screenshots

### Home Page — KPI Overview
Total quotes, shipments, carriers, and alerts sent, with 1-click model retraining and a recent alerts feed.

![Home Page](screenshots/01_home_page.png)

### AI Copilot (Prompt + Response)
Unified AI Copilot answering a live user query using DB stats, port weather, ML scores, and carrier data.

![AI Copilot](screenshots/02_ai_copilot.png)

### ML Pricing Calculator (Input + Predicted Cost)
Agent 1: Global Freight Pricing & Port Congestion — live distance/weight/congestion inputs producing a predicted cost with a 95% confidence interval.

![Pricing Calculator](screenshots/03_pricing_calculator.png)

### Admin Panel — ML Model Card
Champion model, R²/ROC-AUC score, and full algorithm comparison table for each of the 3 agents.

![ML Model Card](screenshots/04_admin_ml_model_card.png)

### Admin Panel — Add / Delete / Unlock User Actions
User Management tab showing Add User, Delete, and the 🔓 Unlock Account action on a temporarily locked user.

![User Management](screenshots/05_admin_add_delete_unlock.png)

### OTP Resend Cooldown Message
Reset Password flow showing the "Please wait 60 seconds before requesting another OTP" cooldown notice.

![OTP Cooldown](screenshots/06a_otp_cooldown.png)

### Triggered Account Lockout Message
Sign-in screen showing "Account temporarily locked for 5 minutes due to 3 failed attempts."

![Lockout Message](screenshots/06b_lockout_message.png)

---

## Final Checklist

- [x] `Milestone2` folder created inside existing Infosys Repository
- [x] README.md added — explains what Milestone 2 adds, includes port coverage table
- [x] Colab runtime set to T4 GPU
- [x] Kaggle API configured (or synthetic fallback confirmed working)
- [x] All secrets set up via Colab Secrets (JWT, admin login, ngrok, HF_TOKEN, optionally email + Kaggle)
- [x] Login / Signup / Forgot Password (OTP) gate all agent & analytics tabs
- [x] Progressive lockout (5/15/permanent) implemented and tested
- [x] OTP resend cooldown (60s / 180s / 300s / 1hr) implemented and tested
- [x] Password strength checker (Weak/Average/Good) implemented on registration and reset
- [x] Agent 1 (Pricing): 5+ algorithms compared, R² ≥ 0.90 confirmed
- [x] Agent 2 (Route Delay): 5+ algorithms compared, ROC-AUC optimized
- [x] Agent 3 (Carrier Compliance): 5+ algorithms compared, ROC-AUC optimized
- [x] AI Copilot returns a real Qwen2.5-3B (4-bit) response and structured JSON audit action
- [x] Admin Dashboard: Add User, Delete User, and Unlock Account all functional
- [x] Admin Panel → ML Model Card tab shows metrics for all 3 agents
- [x] Screenshots captured and linked in README.md
- [x] All personal secrets removed from the notebook before upload
- [x] Notebook restarted, re-run top to bottom, outputs cleared, uploaded as `FreightQuote_AI_Milestone2.ipynb`
