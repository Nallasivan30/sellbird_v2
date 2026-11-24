# Sellbird Startup & Run Commands Guide

This guide contains all the commands to install, configure, start, and manage **Sellbird** on your local machine.

---

## 1. Installation & Environment Setup

Ensure you are in the project root directory (`sellbird`).

```bash
# 1. Create a Python 3.11+ virtual environment
python3 -m venv .venv

# 2. Activate the virtual environment
source .venv/bin/activate

# 3. Install Sellbird and its dependencies in editable mode
pip install -e .

# 4. (Optional) Install Playwright Chromium browser (needed for LinkedIn prospecting)
python -m playwright install chromium
```

> **Tip:** You can also run `sellbird install` to have Sellbird verify Python version, dependencies, and Playwright browsers automatically.

---

## 2. Configuration & API Keys

```bash
# 1. Create your local environment file
cp .env.example .env

# 2. Open .env to add your keys
nano .env   # or code .env
```

### Essential `.env` Settings:
* **Email Provider (pick one):**
  * **Gmail / Google Workspace (Recommended):** Set `GMAIL_CLIENT_ID` and `GMAIL_CLIENT_SECRET`. Then run `sellbird gmail auth` to link your mailbox.
  * **SMTP & IMAP:** Set `SMTP_HOST`, `SMTP_PORT=587`, `SMTP_USERNAME`, `SMTP_PASSWORD`, and `IMAP_HOST`.
* **Email Verifier (Highly recommended to prevent bounces):**
  * `REOON_API_KEY` (600 free validations/month), or `ZEROBOUNCE_API_KEY`, or `HUNTER_API_KEY`.
* **Gemini API Key (for future Gemini integration):**
  * `GEMINI_API_KEY=your_gemini_api_key_here`

---

## 3. Product Training & ICP Definition

Teach Sellbird about your company, your offer, and your ideal customer profile:

```bash
# Option A: Interactive setup wizard
sellbird setup

# Option B: Automatic training from your website (creates sellbird.local.yaml)
sellbird train https://your-company.com

# Confirm free observable buying signals (costs $0, uses no credits)
sellbird signals --confirm free
```

---

## 4. Starting Sellbird

Sellbird consists of two primary runtime components:

### A. The Web Dashboard (UI)
Monitors pipeline health, shows today's actions, and lets you approve outbox drafts.

```bash
# Start the web dashboard (runs at http://127.0.0.1:5555)
sellbird dashboard
```
> The command will output a private link containing an access token:  
> `http://127.0.0.1:5555/#token=...`  
> Open that link in your browser.

### B. The Autonomous Sales Heartbeat Loop
Runs the autonomous SDR cycle every 15 minutes in the background.

```bash
# Start the autonomous heartbeat loop
sellbird run
```

---

## 5. Daily CLI Operations & Manual Commands

You can run individual pipeline actions on demand:

| Action | Command | Purpose |
| :--- | :--- | :--- |
| **Discover Prospects** | `sellbird discover` | Searches for matching businesses (OpenStreetMap by default). |
| **Estimate Discovery Spend** | `sellbird discover --estimate` | Shows projected cost before running discovery queries. |
| **Review Outbox** | `sellbird outbox` | Lists all drafted emails waiting for your manual approval. |
| **Check Health & Stats** | `sellbird status` | Displays system status, provider quota, and pipeline counts. |
| **Check Warmup** | `sellbird warmup` | Checks mailbox warmup progress and readiness. |
| **Pause Sending** | `sellbird sending pause` | Emergency stop: immediately halts all outbound mail. |
| **Resume Sending** | `sellbird sending resume` | Resumes normal outbox drainage. |
| **Export Leads** | `sellbird export --all` | Exports all discovered, profiled, and verified leads to CSV. |
| **Launch MCP Server** | `sellbird mcp` | Starts the stdio Model Context Protocol server for coding agents. |

---

## 6. Running with Docker (Alternative)

To run Sellbird inside Docker containers:

```bash
# 1. Ensure .env is populated
cp .env.example .env

# 2. Build the Docker image
docker compose build

# 3. Start Sellbird in the background
docker compose up -d

# 4. View logs
docker compose logs -f
```
The dashboard will be available at `http://127.0.0.1:5555`.
