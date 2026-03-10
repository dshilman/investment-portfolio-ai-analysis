
# Investment Portfolio Analysis (ChatGPT‑style)

> GitHub repo: https://github.com/dshilman/investement-portfolio-ai-analysis  
> Demo video: https://www.youtube.com/watch?v=-UDMbDFshYk

A lightweight, ChatGPT‑like web application that provides **conversational insights about an investment portfolio**. It builds a local semantic index (via **LlamaIndex**) from live market news (Mboum Finance API) and other sources, then lets you ask natural‑language questions about your holdings—returns, risk, catalysts, diversification, and more. citeturn2search54turn2search40

---

## ✨ What it does
- **Conversational Q&A about your portfolio.** Ask questions in plain English and get synthesized answers grounded in your indexed corpus. citeturn2search54
- **Builds its own knowledge base** from **market news** for a curated set of tickers (default: `AAPL, IBM, TSLA, AMZN`). citeturn2search54
- **Uses Mboum Finance API** for news (and quotes), configured via environment variables. citeturn2search40
- **Runs as a simple Flask app** you can start locally or deploy to a small VM. citeturn2search40

> 📺 See the short video walkthrough here: https://www.youtube.com/watch?v=-UDMbDFshYk

---

## 🧭 Architecture (high level)
```
User → Flask (app.py) → LlamaIndex (VectorStoreIndex) → Local index on disk
                               ↘ Data builders (create_index_from_api.py, etc.) → Mboum Finance API
```
- **Index builders**: `create_index_from_api.py` pulls **news** via RapidAPI, turns each article into a `Document`, splits into **nodes**, and persists a **VectorStoreIndex** on disk. citeturn2search54
- **Environment config**: `.env` carries the app mode plus keys/hosts/URLs for **OpenAI** and **Mboum Finance**. citeturn2search40
- **Runtime**: the Flask app loads the stored index and answers questions against it in a ChatGPT‑style interface. *(Repo contains `app.py` and `templates/` for the UI.)* citeturn2search40

---

## 📁 Repository layout
```
.
├─ app.py                         # Flask entrypoint (chat-like UI)
├─ templates/                     # Jinja templates for the web UI
├─ create_index_from_api.py       # Build index from Mboum news
├─ create_index_from_file.py      # (Optional) Build index from local files
├─ create_index_from_web.py       # (Optional) Build index by crawling web pages
├─ .env                           # Example env (keys/hosts/URLs)
├─ requirements.txt               # Python deps (Flask, LlamaIndex, etc.)
└─ linux.sh                       # Convenience install script for Linux
```
*(See the repo tree for exact files and paths.)* citeturn2search40turn2search54turn2search61turn2search62

---

## 🔑 Prerequisites
- **Python 3.9+**
- Accounts/keys for:
  - **OpenAI** (`OPENAI_API_KEY`) – used by LlamaIndex for LLM calls. citeturn2search40
  - **Mboum Finance (RapidAPI)** – `X-RapidAPI-Key`, `X-RapidAPI-Host`, plus `NEWS_API_URL` and optional quote endpoint. citeturn2search40

---

## ⚙️ Setup & Run (local)
1) **Clone and install**
```bash
python3 -m venv .venv && source .venv/bin/activate
python -m pip install -r requirements.txt
```
`requirements.txt` includes **llama-index**, **flask**, **python-dotenv**, and optional extras. citeturn2search62

2) **Configure environment** (create `.env` in repo root)
```ini
FLASK_APP=app.py
FLASK_ENV=development
FLASK_DEBUG=True
OPENAI_API_KEY=...your key...
X-RapidAPI-Key=...your key...
X-RapidAPI-Host=mboum-finance.p.rapidapi.com
QUOTE_API_URL=https://mboum-finance.p.rapidapi.com/qu/quote
NEWS_API_URL=https://mboum-finance.p.rapidapi.com/ne/news/
```
*(These names/values mirror the example in the repo.)* citeturn2search40

3) **Build the knowledge index** from market news
```bash
python create_index_from_api.py
```
- Pulls latest articles for default tickers, converts to `Document` objects, chunks to **nodes**, and **persists** a `VectorStoreIndex` under `./indexed_files/api_index`. citeturn2search54

4) **Start the app**
```bash
python app.py
```
- Launches a Flask server and serves a simple chat UI (templates included). citeturn2search40

---

## 🧪 Prompting examples
- *“What are the top risks and catalysts mentioned for TSLA this week?”* (Grounded in indexed news.) citeturn2search54
- *“Summarize today’s headlines across AAPL/AMZN and the likely impact on my portfolio risk.”* citeturn2search54

> Tip: Re‑run the index builder periodically (or on a schedule) to keep answers fresh. You can wire this to a cron job or GitHub Action later. citeturn2search39

---

## 🚀 Deployment options
### Quick VM (Linux)
The repo includes a tiny helper script that installs pip, cron, and dependencies. Adapt it to your distro and process manager (systemd). citeturn2search61
```bash
./linux.sh
```

### GitHub Actions to EC2 (example)
There’s a **Push‑to‑DEMO‑EC2** workflow history in Actions—use it as inspiration to rsync/SSH your build to a small EC2 instance. citeturn2search39

---

## 🔒 Security & data notes
- Do **not** commit your API keys. Use `.env` locally and **GitHub Secrets**/key vaults in CI/CD. citeturn2search40
- Answers are only as current as the data in your local index—schedule rebuilds for up‑to‑date insights. citeturn2search54

---

## 🧱 Tech stack
- **Flask** (web server & templates). citeturn2search40  
- **LlamaIndex** (ingestion, node parsing, vector store). citeturn2search54turn2search62  
- **OpenAI API** (LLM calls behind the scenes). citeturn2search40  
- **Mboum Finance API via RapidAPI** (news/quotes). citeturn2search40

---

## 📹 Demo
- Watch: https://www.youtube.com/watch?v=-UDMbDFshYk (Investment Portfolio Analysis overview)

---

## 🙌 Credits
- Built by **David Shilman**. Repo and workflows referenced above. citeturn2search36turn2search39

