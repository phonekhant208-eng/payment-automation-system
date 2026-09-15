# ⚡ Automated Payment Verification & Code Activation System

An automated backend service built with **n8n**, **Groq Vision AI**, and **Supabase**. This system receives payment receipt screenshots via a Telegram bot, extracts transaction details using Vision OCR, executes multi-stage fraud checks, and delivers single-use activation codes.

---

## 📐 System Architecture

![n8n Payment Workflow](https://github.com/user-attachments/assets/a34725fe-2f5c-4e97-a823-ed95a10d8bc1)



### Workflow Breakdown

1. **Telegram Listener:** Listens for incoming payment receipt screenshots and user commands via `Telegram Trigger`.
2. **File Processing & OCR:** Downloads receipt images, converts them to base64, and queries the **Groq AI Vision API** (`llama-3.2-11b-vision-preview`) to extract Transaction ID, Amount, Timestamp, and Recipient Info.
3. **Multi-Condition Fraud Detection:**
   Validates extracted receipt data against **4 mandatory verification checks**:
   * **Transaction ID Uniqueness:** Queries Supabase (`Get many rows`) to ensure the receipt hasn't been used before (prevents double-spending).
   * **Recipient Phone Number:** Verifies the last 4 digits of the recipient mobile number match the merchant target.
   * **Exact Account Name:** Confirms the recipient account name matches the target account exactly.
   * **Payment Amount:** Validates that the transferred amount matches the required fee.
4. **Collision Handling & Admin Alerts:**
   * Generates a random activation key and checks for duplicates in the database.
   * **Collision Prevention:** If a generated key matches an existing record in Supabase, the workflow halts activation and sends an instant Telegram alert to the admin (*"Duplicated code detected"*).
5. **Activation & Delivery:** Records the verified activation in Supabase (`Create a row`) and sends the activation key directly to the user via Telegram (`Send a text message`).

---

## 🛠️ Tech Stack

* **Automation Engine:** [n8n](https://n8n.io/) (Self-hosted on Docker / Ubuntu)
* **OCR / Vision AI:** Groq API (`llama-3.2-11b-vision-preview`)
* **Database & Security:** [Supabase](https://supabase.com/) (PostgreSQL with RLS & RPC functions)
* **Interface:** Telegram Bot API
* **Frontend Application:** [View Frontend Repository](https://github.com/phonekhant208-eng/BitByBitWebsite)

---

## 🔒 Environment Variables & Credentials

Configure the following credentials within your n8n credentials manager:

| Credential Name | Service | Purpose |
| :--- | :--- | :--- |
| `telegramApi` | Telegram Bot | Receives receipt slips & sends user/admin messages |
| `groqApi` | Groq AI | Vision OCR for receipt slip parsing |
| `supabaseApi` | Supabase DB | Verification check & code activation storage |

> **Security Note:** Never commit raw API keys, active database secrets, or webhook URLs directly to GitHub repository files.

---

## 🚀 How to Import & Run

1. Clone this repository:
   ```bash
   git clone [https://github.com/phonekhant208-eng/kbzpay-n8n-backend.git](https://github.com/phonekhant208-eng/kbzpay-n8n-backend.git)
