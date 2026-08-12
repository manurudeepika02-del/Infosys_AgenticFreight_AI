# 🏛️ Infosys Agentic AI Portal — Milestone 1

**Login · Signup · Forgot Password** system built with **Streamlit**, running on **Google Colab**, exposed publicly via **ngrok**. Uses **JWT-based session handling** and sends a **Gmail OTP** for secure password recovery.

---

## Features

- User Signup with security question fallback
- Secure Login with credential validation
- Forgot Password flow with email-based OTP (6-digit, expires in 5 minutes)
- JWT-based session handling
- Public access via ngrok tunnel (not limited to localhost)
- Enterprise-style analytics dashboard after login

---

## Tech Stack

| Component        | Technology         |
|-------------------|---------------------|
| UI Framework      | Streamlit           |
| Hosting            | Google Colab        |
| Public Exposure    | ngrok                |
| Session Handling   | JWT                  |
| OTP Delivery       | Gmail (SMTP)         |
| Charts             | Plotly               |

---

## Setup

1. **Create an ngrok account** at [ngrok.com](https://ngrok.com) and get your authtoken.
2. **Enable 2-Step Verification** on the Gmail account used to send OTPs, then generate a **Gmail App Password**.
3. Store the following as Colab Secrets (do not hardcode credentials in the notebook):
   - `NGROK_AUTHTOKEN`
   - `EMAIL_ID` / `EMAIL_PASSWORD` (Gmail App Password)
   - `JWT_SECRET`
4. Run all cells in the notebook top to bottom.
5. Ngrok will output a public URL — open it in your browser to access the portal.

---

## Screenshots

### 1. Login Page
![Login Page](screenshots/login_page.png)

### 2. Login — Invalid Credentials
![Invalid Login](screenshots/login_invalid.png)

### 3. Login — Success → Dashboard
![Dashboard](screenshots/dashboard.png)

### 4. Signup — Create Account
![Signup Form](screenshots/signup_form.png)

### 5. Signup — Account Created
![Account Created](screenshots/signup_success.png)

### 6. Forgot Password — OTP Entry
![OTP Entry](screenshots/otp_entry.png)

### 7. OTP Email Received
![OTP Email](screenshots/otp_email.png)

### 8. Password Reset — Success
![Password Reset Success](screenshots/password_reset_success.png)

> Replace the image paths above with your actual screenshot files (place them in a `screenshots/` folder alongside this README, or update the paths to wherever you upload them for submission).

---

## Notes

- All secrets (Gmail app password, JWT secret, ngrok token) should be stored via Colab Secrets, **not** hardcoded in the notebook.
- The ngrok free tier issues a new random URL on every restart unless a reserved domain is configured.
