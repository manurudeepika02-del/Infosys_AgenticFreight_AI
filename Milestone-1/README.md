# 🏛️ Infosys Freight Quote Portal

### Milestone 1 — Authentication System & Enterprise Dashboard

A secure, professional-grade web portal built with **Streamlit**, featuring a complete authentication system (login, signup, password recovery via OTP/security question) and role-based dashboards for Admin and User accounts.

---

## 📌 Project Overview

The **Infosys Freight Quote Portal** is designed to serve as the foundation for a freight quotation and analytics platform. Milestone 1 focuses on establishing a robust, secure, and visually professional entry point into the system — covering user identity management and a preliminary analytics dashboard.

---

## ✅ Milestone 1 — Scope & Deliverables

| Module | Status |
|---|---|
| User Registration (Sign Up) | ✅ Completed |
| Secure Login with JWT Sessions | ✅ Completed |
| Password Recovery — Security Question | ✅ Completed |
| Password Recovery — Email OTP | ✅ Completed |
| Role-Based Access (Admin / User) | ✅ Completed |
| Admin Control Panel (base view) | ✅ Completed |
| User Analytics Dashboard | ✅ Completed |
| Professional Navy & Gold UI Theme | ✅ Completed |
| Local Deployment via Ngrok (Colab) | ✅ Completed |

---

## 🎯 Key Features

### 🔐 Authentication & Security
- **Secure password storage** using `bcrypt` hashing (passwords and security answers are never stored in plain text)
- **JWT-based session management** with 2-hour token expiry
- **Two independent password recovery paths:**
  - Security question verification
  - One-Time Password (OTP) sent via email (Gmail SMTP), valid for 5 minutes with resend support
- Field-level validation with clear, specific error messaging on signup

### 👥 Role-Based Dashboards
- **Admin Dashboard** — dedicated control panel view for the administrator account
- **User Dashboard** — enterprise analytics view featuring:
  - Key metric cards (Documents Indexed, Searches Today, Efficiency Score, Security Status)
  - Interactive System Health gauge chart (Plotly)

### 🎨 Design System
- Custom **Navy & Gold** professional theme built for enterprise presentation
- Typography: **Playfair Display** (headings) + **Inter** (body) for a classic, corporate aesthetic
- Fully custom-styled inputs, buttons, sidebar navigation, and metric cards
- Responsive wide-layout design suitable for desktop enterprise use

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend / App Framework | [Streamlit](https://streamlit.io/) |
| Authentication | `PyJWT`, `bcrypt` |
| Database | SQLite3 (local file-based storage) |
| Email / OTP Delivery | `smtplib` (Gmail SMTP) |
| Data Visualization | `Plotly` |
| Navigation Component | `streamlit-option-menu` |
| Tunneling / Deployment | `pyngrok` (Google Colab environment) |

---

## 📂 Project Structure

```
Milestone-1/
│
├── Login_Page.ipynb        # Main project notebook (Streamlit app + Colab deployment)
├── README.md                # Project documentation (this file)
└── LICENSE                  # License information
```

> Note: `app.py`, `infosys_portal.db`, and `.streamlit/config.toml` are generated automatically at runtime when `Login_Page.ipynb` is executed — they are not committed to the repository.

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.10+
- A Gmail account with an [App Password](https://myaccount.google.com/apppasswords) enabled (for OTP email delivery)

### Installation

```bash
pip install streamlit streamlit-option-menu pyngrok pyjwt bcrypt plotly
```

### Configuration

Set your email credentials as environment variables before running:

```bash
export SMTP_EMAIL="your-email@gmail.com"
export SMTP_APP_PASSWORD="your-16-character-app-password"
```

> ⚠️ Never commit real credentials to source control. Use environment variables or a secrets manager (e.g. Colab Secrets) in all environments.

### Run the App

**Option A — Google Colab (recommended, as used in this project):**
1. Open `Login_Page.ipynb` in Google Colab
2. Run the cells in order:
   - **Cell 1:** installs dependencies
   - **Cell 2:** writes the Streamlit application (`app.py`) to disk
   - **Cell 3:** launches Streamlit and exposes it via a public `ngrok` URL
3. Click the generated public URL to access the live portal

**Option B — Local machine:**
```bash
streamlit run app.py
```
The app will be available at `http://localhost:8501`.

---

## 👤 Default Credentials

| Role | Email | Password |
|---|---|---|
| Administrator | `infosys@ai` | `admin@123` |

> It's recommended to change the default admin password after first login in a production setting.

---

## 🗺️ Roadmap — Upcoming Milestones

- [ ] Freight quote request & calculation engine
- [ ] Shipment tracking integration
- [ ] Advanced analytics & reporting module
- [ ] Multi-user role hierarchy (Manager, Operator, Viewer)
- [ ] REST API layer for third-party integrations
- [ ] Migration from SQLite to a production-grade database (PostgreSQL)

---

## 📄 License

This project is developed as part of an internship program under Infosys Springboard. All rights reserved to the respective organization/internship program.

---

## 🙋 Support

For issues or questions regarding this milestone, please reach out to the project mentor or raise an issue in the internal tracking system.

## ScreenShots :

## Sign In
<img width="1917" height="972" alt="Screenshot 2026-07-08 162644" src="https://github.com/user-attachments/assets/e97d1a98-4cf4-44c5-b57a-8a216fb9f5b0" />

## Sign Up
<img width="1917" height="973" alt="Screenshot 2026-07-08 162807" src="https://github.com/user-attachments/assets/90687be1-71dc-4642-bcba-2fc604a3bde1" />

## User DashBoard
<img width="1912" height="891" alt="Screenshot 2026-07-08 162740" src="https://github.com/user-attachments/assets/1b1004e2-1b63-4a50-970e-09ac7de4f0e1" />

## Forgot Password
<img width="1910" height="967" alt="Screenshot 2026-07-08 162823" src="https://github.com/user-attachments/assets/00bc9716-6d16-4ea2-8a94-646bec71f017" />

## Email Confirmation Through OTP
<img width="1501" height="557" alt="Screenshot 2026-07-08 162931" src="https://github.com/user-attachments/assets/b7395f2a-16e0-4a5e-bdcb-f47c69ae80fa" />

## Reset Password
<img width="1917" height="951" alt="Screenshot 2026-07-08 162846" src="https://github.com/user-attachments/assets/3b853133-ef5d-4079-a4fc-abf1b81fc07f" />




