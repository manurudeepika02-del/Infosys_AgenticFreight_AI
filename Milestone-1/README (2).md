# 🏛️ Infosys Agentic AI Portal — Milestone 1

## Overview

This repository contains the **Milestone 1** deliverable for the Infosys Springboard internship project **Agentic AI for Maritime Freight Pricing and Route Optimization**.

Milestone 1 establishes the **authentication layer** of the platform — a secure, production-style **Login · Signup · Forgot Password** module that will serve as the entry point for the full Agentic AI system in later milestones. The application is built entirely with **Streamlit**, executed inside a **Google Colab** runtime, and exposed to the public internet through an **ngrok** tunnel so it can be accessed and evaluated from any browser.

The module was designed with real-world authentication practices in mind: hashed credentials, signed session tokens, and email-based one-time-password (OTP) verification for password recovery — rather than a toy login form.

---

## Objective

> Build a complete Login · Signup · Forgot Password system using Streamlit, running on Google Colab, exposed publicly via ngrok. It must include JWT-based session handling and send a Gmail OTP for secure password recovery.

This README documents how that objective was met, the architecture behind it, and how to reproduce and evaluate the running application.

---

## Key Features

| Feature | Description |
|---|---|
| **User Signup** | Collects full name, email, password (min. 8 characters, confirmed), and a security question/answer pair as a secondary recovery method. Rejects duplicate emails/usernames. |
| **Secure Login** | Validates credentials against hashed passwords stored in SQLite; issues a signed JWT on success. |
| **Forgot Password — Security Question** | Lets a user reset their password by correctly answering their pre-set security question. |
| **Forgot Password — Email OTP** | Sends a 6-digit numeric OTP to the user's registered Gmail address via SMTP. OTP expires after 5 minutes. |
| **JWT Session Handling** | On successful login/signup, a JWT (`HS256`, 2-hour expiry) is generated and stored in Streamlit session state, gating access to the dashboard. |
| **Password Hashing** | All passwords and security answers are hashed with `bcrypt` before storage — nothing is stored in plain text. |
| **Public Deployment** | The Streamlit app is tunnelled through `ngrok`, producing a public HTTPS URL so the app can be tested outside the Colab environment. |
| **Enterprise Dashboard (post-login)** | A themed analytics landing page (navy & gold, Plotly gauge chart, KPI cards) confirms the authenticated session is working end-to-end. |

---

## Architecture

```
┌─────────────────────┐
│   Google Colab       │  ← Notebook execution environment
│  ┌────────────────┐  │
│  │  app.py          │  │  ← Streamlit application (written via %%writefile)
│  │  (Streamlit)     │  │
│  └───────┬─────────┘  │
│          │ port 8501   │
│  ┌───────▼─────────┐  │
│  │  ngrok tunnel    │  │  ← Exposes local Streamlit server publicly
│  └───────┬─────────┘  │
└──────────┼────────────┘
           │
     Public HTTPS URL  ──►  Browser (any device)
           │
   ┌───────▼────────┐
   │ SQLite database │  ← Stores users: id, username, email,
   │ (infosys_portal │     password_hash, security_question,
   │      .db)        │     security_answer_hash
   └────────────────┘
           │
   ┌───────▼────────┐
   │ Gmail SMTP      │  ← Sends OTP emails for password reset
   └────────────────┘
```

**Authentication flow:**
1. User submits credentials on the Login page.
2. Password is verified against the `bcrypt` hash stored in SQLite.
3. On success, a JWT is signed (email + 2-hour expiry) and kept in `st.session_state.token`.
4. The presence of a valid token gates access to the Dashboard; without it, the user is redirected back to Login.

**Password recovery flow (OTP path):**
1. User enters their registered email on the Forgot Password screen and selects "Via OTP."
2. A random 6-digit code is generated and emailed via Gmail SMTP (`smtplib` + `MIMEText`).
3. The code and its expiry timestamp are held in session state (5-minute validity).
4. User enters the OTP plus a new password; on match, the password hash is updated in the database.

---

## Tech Stack

| Layer | Technology |
|---|---|
| UI Framework | Streamlit + `streamlit-option-menu` |
| Execution Environment | Google Colab |
| Public Exposure | ngrok (`pyngrok`) |
| Authentication Token | JWT (`pyjwt`, HS256) |
| Password Hashing | `bcrypt` |
| Database | SQLite3 |
| OTP Delivery | Gmail SMTP (`smtplib`) |
| Charts / Dashboard | Plotly |

---

## Database Schema

```sql
CREATE TABLE users (
    id                    INTEGER PRIMARY KEY AUTOINCREMENT,
    username              TEXT UNIQUE,
    email                 TEXT UNIQUE,
    password_hash         TEXT,
    security_question     TEXT,
    security_answer_hash  TEXT
);
```

A default administrator account is seeded on first run for testing.

---

## Setup & Run Instructions

1. **Create an ngrok account** at [ngrok.com](https://ngrok.com) and copy your authtoken.
2. **Enable 2-Step Verification** on the Gmail account used to send OTPs, then generate a **Gmail App Password** from Google Account → Security → App Passwords.
3. In the Colab notebook, open **Secrets** (🔑 icon in the left sidebar) and add:
   - `NGROK_AUTHTOKEN` — your ngrok token
   - `EMAIL_ID` — the sender Gmail address
   - `EMAIL_PASSWORD` — the Gmail App Password (never hardcode this in the notebook)
   - `JWT_SECRET` — a private signing key for JWTs
4. Run all notebook cells sequentially:
   - Cell 1 installs dependencies (`streamlit`, `pyngrok`, `pyjwt`, `bcrypt`, `plotly`, etc.)
   - Cell 2 writes the full Streamlit app to `app.py`
   - Cell 3 launches Streamlit in the background and opens the ngrok tunnel
5. The tunnel cell prints a public URL, e.g. `https://<random-subdomain>.ngrok-free.dev` — open it in any browser to access the live portal.
6. To stop the app cleanly, interrupt the running cell (Ctrl+C / Colab stop button); this kills the ngrok tunnel and the Streamlit process.

---

## Security Notes

- Passwords and security answers are never stored in plain text — both are hashed with `bcrypt` before being written to SQLite.
- Session state is gated by a signed, time-limited JWT rather than a simple boolean flag.
- All secrets (ngrok token, Gmail credentials, JWT signing key) should be stored via **Colab Secrets**, not hardcoded in the notebook source.
- The free ngrok tier issues a new random subdomain on every restart unless a reserved/static domain is configured.

---

## Screenshots

### 1. Login Page
<img width="1920" height="1080" alt="Screenshot (70)" src="https://github.com/user-attachments/assets/f893d018-3c2e-49db-bf0f-0f24a356c7f4" />


### 2. Login — Invalid Credentials
<img width="1920" height="1080" alt="Screenshot (65)" src="https://github.com/user-attachments/assets/14873c96-910e-4d28-9cb4-626b97e37f7d" />


### 3. Login — Success → Dashboard
<img width="1920" height="1080" alt="Screenshot (66)" src="https://github.com/user-attachments/assets/6f215226-b6f9-4d5d-8e4b-e7622eaf7677" />


### 4. Signup — Create Account
<img width="1920" height="1080" alt="Screenshot (73)" src="https://github.com/user-attachments/assets/309b3652-bb17-4c68-907d-221bfe53c882" />


### 5. Signup — Account Created
<img width="1920" height="1080" alt="Screenshot (75)" src="https://github.com/user-attachments/assets/0761dd72-0cc4-4219-babb-71a32099bd94" />


### 6. Forgot Password — OTP Entry
<img width="1920" height="1080" alt="Screenshot (67)" src="https://github.com/user-attachments/assets/357fc263-3407-41c0-9094-fab4cb3d8280" />


### 7. OTP Email Received
<img width="1920" height="1080" alt="Screenshot (71)" src="https://github.com/user-attachments/assets/c571bfdf-8002-48f4-9339-666cb385a1ca" />


### 8. Password Reset — Success
<img width="1920" height="1080" alt="Screenshot (72)" src="https://github.com/user-attachments/assets/fcea17f0-2512-4a97-8f88-66365a401795" />


> Place your screenshot files in a `screenshots/` folder alongside this README (or update the paths above to wherever they're hosted for submission).

---

## Project Structure

```
.
├── Login_Page.ipynb        # Colab notebook: installs deps, writes app.py, launches app + ngrok
├── app.py                  # Generated Streamlit application (written by the notebook)
├── infosys_portal.db       # SQLite database (created at runtime)
├── screenshots/            # Evidence screenshots for this milestone
└── README.md
```

---

## Roadmap — Beyond Milestone 1

This authentication module is the foundation for the broader **Agentic AI for Maritime Freight Pricing and Route Optimization** system. Subsequent milestones will build on the authenticated session established here to add:
- Freight quote generation and pricing agents
- Route optimization logic
- Integration of the agentic pipeline behind the authenticated dashboard

---

## Author

**Manuru Deepika** — Infosys Springboard Internship
