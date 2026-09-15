# ⚡ Automated Payment Verification & Code Activation System

An automated backend service built with **n8n**, **Groq Vision AI**, and **Supabase**. This system captures payment receipt screenshots via a Telegram bot, extracts transaction details using Vision OCR, validates payments against a Supabase database to prevent double-spending, and automatically delivers single-use activation codes to users.

---

## 📐 System Architecture

![n8n Payment Workflow](./workflow-diagram.png)

### Workflow Breakdown
1. **Telegram Listener:** Listens for incoming user payment slips and commands via `Telegram Trigger`.
2. **File Processing & OCR:** Downloads receipt images, converts them to base64, and sends them to the **Groq AI Vision API** (`llama-3.2-11b-vision-preview`) for instant OCR extraction (Transaction ID, Amount, Timestamp).
3. **Fraud Detection & DB Check:** Queries the **Supabase** database (`Get many rows`) to verify if the Transaction ID has already been claimed or if the amount matches the required fee.
4. **Logic Evaluation:** Executes JavaScript validation logic (`If` nodes) to confirm transaction authenticity.
5. **Activation & Response:** Generates an activation record in Supabase (`Create a row`) and dispatches the activation key back to the user via Telegram (`Send a text message`).

---

## 🛠️ Tech Stack

* **Automation Engine:** [n8n](https://n8n.io/) (Self-hosted on Docker / Ubuntu)
* **OCR / Vision AI:** Groq API (`llama-3.2-11b-vision-preview`)
* **Database & Auth:** [Supabase](https://supabase.com/) (PostgreSQL with Row Level Security)
* **Communication Interface:** Telegram Bot API
* **Frontend Integration:** [View Frontend Repository](https://github.com/phonekhant208-eng/BitByBitWebsite)

---

## 🔒 Environment Variables & Credentials Setup

To run this workflow, configure the following secrets within your n8n credentials manager:

| Credential Name | Service | Purpose |
| :--- | :--- | :--- |
| `telegramApi` | Telegram Bot | Receives slips & sends activation codes |
| `groqApi` | Groq AI | Vision OCR for payment slip parsing |
| `supabaseApi` | Supabase DB | Payment verification & activation code storage |

> **Note:** Never commit raw API keys or active database credentials to GitHub.

---

## 🚀 How to Import & Run

1. Clone this repository:
   ```bash
   git clone [https://github.com/phonekhant208-eng/kbzpay-n8n-backend.git](https://github.com/phonekhant208-eng/kbzpay-n8n-backend.git)
