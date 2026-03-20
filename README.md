<div align="center">

# 🩺 MediBot — AI Health Intelligence Assistant

### *Your intelligent companion for health information, medical research & document analysis*

[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.35%2B-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](https://streamlit.io)
[![Groq](https://img.shields.io/badge/Groq-Llama3--70B-F55036?style=for-the-badge)](https://console.groq.com)
[![ChromaDB](https://img.shields.io/badge/ChromaDB-Vector%20Store-6C3483?style=for-the-badge)](https://www.trychroma.com)
[![License](https://img.shields.io/badge/License-MIT-00d4aa?style=for-the-badge)](LICENSE)

<br/>

> **Built for the NeoStats AI Engineer Case Study**
> *"Tools don't solve problems. People do."*

<br/>

🌐 **[Live Demo →](https://your-app.streamlit.app)** &nbsp;|&nbsp; 📂 **[Project ZIP →](#)** &nbsp;|&nbsp; 📊 **[Presentation →](#)**

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Use Case](#-use-case)
- [Features](#-features)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Quick Start](#-quick-start)
- [API Keys Setup](#-api-keys-setup)
- [Deployment](#-deployment-streamlit-cloud)
- [How It Works](#-how-it-works)
- [Screenshots](#-screenshots)
- [Challenges & Solutions](#-challenges--solutions)
- [Author](#-author)

---

## 🎯 Overview

**MediBot** is a production-grade AI chatbot built for the healthcare & medical education domain. It goes beyond a simple LLM wrapper — it combines **Retrieval-Augmented Generation (RAG)**, **live web search**, **multi-LLM support**, and an intelligent response-mode system to deliver accurate, contextual, and trustworthy health information.

Unlike generic chatbots, MediBot:
- References **your uploaded medical documents** (PDFs, research papers, clinical guidelines)
- Fetches **real-time data** from the web when queries need the latest information
- Adapts its **communication style** (Concise / Detailed / ELI5) to the user's needs
- Automatically flags **medical emergencies** with Indian helpline numbers
- Tracks **session analytics** to understand query patterns

---

## 🏥 Use Case

Healthcare information is overwhelming, jargon-heavy, and often outdated. MediBot bridges the gap for:

| User | Problem | MediBot Solution |
|---|---|---|
| Medical Students | Can't find quick, reliable answers during study | RAG over uploaded textbooks & notes |
| Researchers | Need latest clinical trial data | Live web search for real-time results |
| Patients | Can't understand medical reports | ELI5 mode — plain language explanations |
| Clinicians | Need fast drug/guideline lookups | RAG over uploaded protocols + detailed mode |

---

## ✅ Features

### Mandatory Features *(NeoStats Brief)*

| # | Feature | Implementation |
|---|---|---|
| 1 | **RAG Integration** | ChromaDB + sentence-transformers (`all-MiniLM-L6-v2`) — local, free, no API key |
| 2 | **Live Web Search** | Tavily API (primary) → DuckDuckGo (free fallback, zero config) |
| 3 | **Response Modes** | ⚡ Concise · 📋 Detailed — sidebar radio toggle, injected into system prompt |
| 4 | **Multi-LLM Support** | OpenAI GPT-4o-mini · Groq Llama3-70B · Google Gemini 1.5 Flash |
| 5 | **Modular Structure** | `config/` · `models/` · `utils/` — clean separation of concerns |
| 6 | **Secure API Keys** | All keys via `os.getenv()` — never hard-coded |

### Additional / Innovative Features

| Feature | Description |
|---|---|
| 🧒 **ELI5 Mode** | Third response mode — zero jargon, everyday analogies |
| 🚨 **Emergency Detection** | Scans every query for critical keywords (chest pain, overdose, stroke) → shows Indian emergency numbers (112, iCall 9152987821) |
| 📊 **Session Analytics** | Live dashboard: total queries, RAG hit rate, web search count, query category breakdown |
| ⬇️ **Chat Export** | Download full conversation as formatted `.txt` |
| 🌡️ **Creativity Slider** | Real-time temperature control (0.0 – 1.0) |
| 🔖 **Source Badges** | Every response shows `RAG` and/or `Web` badges — full transparency |
| 🔄 **Streaming Responses** | Token-by-token streaming with blinking cursor indicator |
| 📎 **Multi-format Upload** | PDF · TXT · DOCX · MD — all indexed into ChromaDB |
| 🗄️ **KB Management** | Clear knowledge base with one click |
| 📈 **Query Classification** | Auto-categorises: symptom · medication · research · nutrition · mental\_health |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        USER QUERY                               │
└──────────────────────────┬──────────────────────────────────────┘
                           │
              ┌────────────▼────────────┐
              │    app.py (Orchestrator) │
              │    Streamlit UI + Logic  │
              └──┬──────────────────┬───┘
                 │                  │
    ┌────────────▼───┐    ┌────────▼────────────┐
    │  RAG Pipeline  │    │  Web Search Pipeline │
    │                │    │                      │
    │ utils/rag_utils│    │ utils/web_search.py  │
    │  ┌──────────┐  │    │  ┌────────────────┐  │
    │  │ChromaDB  │  │    │  │ Tavily API     │  │
    │  │Vector DB │  │    │  │ (primary)      │  │
    │  └──────────┘  │    │  └────────────────┘  │
    │  ┌──────────┐  │    │  ┌────────────────┐  │
    │  │sentence- │  │    │  │ DuckDuckGo     │  │
    │  │transform │  │    │  │ (free fallback)│  │
    │  └──────────┘  │    │  └────────────────┘  │
    └────────────────┘    └──────────────────────┘
                 │                  │
              ┌──▼──────────────────▼───┐
              │    Context Builder       │
              │    utils/chat_utils.py   │
              │  System Prompt + History │
              │  + RAG Context           │
              │  + Web Context           │
              └──────────┬──────────────┘
                         │
              ┌──────────▼──────────────┐
              │      LLM Provider        │
              │    models/llm.py         │
              │                          │
              │  OpenAI │ Groq │ Gemini  │
              └──────────┬──────────────┘
                         │
              ┌──────────▼──────────────┐
              │   Streamed Response      │
              │   + Source Badges        │
              │   + Analytics Update     │
              └─────────────────────────┘
```

---

## 📁 Project Structure

```
medibot/
│
├── config/
│   ├── __init__.py
│   └── config.py              ← All API keys & settings (env vars only)
│
├── models/
│   ├── __init__.py
│   ├── llm.py                 ← OpenAI / Groq / Gemini streaming adapters
│   └── embeddings.py          ← SentenceTransformer + ChromaDB EmbeddingFunction
│
├── utils/
│   ├── __init__.py
│   ├── rag_utils.py           ← Document load → chunk → embed → index → retrieve
│   ├── web_search.py          ← Tavily primary + DuckDuckGo fallback
│   ├── chat_utils.py          ← Prompt builder, emergency detection, export
│   └── analytics.py           ← Session-level query analytics tracker
│
├── app.py                     ← Main Streamlit UI — orchestrates all modules
│
├── requirements.txt           ← All dependencies
├── .env.example               ← Template for API keys (copy → .env)
├── .gitignore                 ← Excludes .env, venv, chroma_db, __pycache__
└── .streamlit/
    └── config.toml            ← Dark teal theme for Streamlit Cloud
```

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | Streamlit 1.35+ | Chat UI, sidebar controls, streaming |
| **LLM — Groq** | Llama3-70B (default) | Fast, free inference |
| **LLM — OpenAI** | GPT-4o-mini | Optional premium model |
| **LLM — Gemini** | Gemini 1.5 Flash | Optional Google model |
| **Embeddings** | sentence-transformers | Local HuggingFace model — no API needed |
| **Vector Store** | ChromaDB | Persistent cosine-similarity search |
| **PDF Parsing** | PyMuPDF (fitz) | Extract text from medical PDFs |
| **DOCX Parsing** | python-docx | Extract text from Word documents |
| **Web Search** | Tavily API | Semantic medical-aware search |
| **Web Fallback** | DuckDuckGo | Free search, zero configuration |
| **Config** | python-dotenv | Secure environment variable loading |

---

## 🚀 Quick Start

### Prerequisites
- Python 3.10 or higher
- pip
- At least one LLM API key (Groq is free — see [API Keys Setup](#-api-keys-setup))

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/medibot.git
cd medibot
```

### 2. Create a virtual environment

```bash
python -m venv venv

# Activate — macOS/Linux:
source venv/bin/activate

# Activate — Windows:
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

> **Note:** First run downloads the `all-MiniLM-L6-v2` embedding model (~90 MB). This only happens once and is cached locally.

### 4. Set up API keys

```bash
cp .env.example .env
```

Open `.env` and add your keys:

```env
# Required — at least one LLM key
GROQ_API_KEY=gsk_your_groq_key_here

# Optional
OPENAI_API_KEY=sk-proj-your_openai_key_here
GEMINI_API_KEY=AIzaSy_your_gemini_key_here
TAVILY_API_KEY=tvly-your_tavily_key_here
```

### 5. Run the app

```bash
streamlit run app.py
```

Open your browser at **http://localhost:8501** 🎉

---

## 🔑 API Keys Setup

| Key | Where to Get | Cost | Required? |
|---|---|---|---|
| `GROQ_API_KEY` | [console.groq.com](https://console.groq.com) | **Free** | ✅ Yes (recommended) |
| `OPENAI_API_KEY` | [platform.openai.com](https://platform.openai.com) | Pay-per-use (~$0.001/query) | ❌ Optional |
| `GEMINI_API_KEY` | [aistudio.google.com](https://aistudio.google.com) | Free tier (1M tokens/day) | ❌ Optional |
| `TAVILY_API_KEY` | [tavily.com](https://tavily.com) | Free tier (1000 searches/month) | ❌ Optional |

> **Minimum to run:** Only `GROQ_API_KEY` is needed. Get it free in 2 minutes at [console.groq.com](https://console.groq.com) — no credit card required.

> **Web Search without Tavily:** If `TAVILY_API_KEY` is not set, MediBot automatically falls back to **DuckDuckGo** (free, no key needed). Web search always works.

---

## ☁️ Deployment (Streamlit Cloud)

Deploy for free in under 5 minutes:

### Step 1 — Push to GitHub

```bash
git add .
git commit -m "Initial MediBot deployment"
git push origin main
```

> Make sure `.env` is listed in `.gitignore` — **never push your API keys to GitHub.**

### Step 2 — Connect to Streamlit Cloud

1. Go to [streamlit.io/cloud](https://streamlit.io/cloud)
2. Sign in with GitHub
3. Click **New App**
4. Select your repository → set **Main file path** to `app.py`
5. Click **Advanced settings**

### Step 3 — Add Secrets

In the **Secrets** field, paste your keys in TOML format:

```toml
GROQ_API_KEY = "gsk_your_key_here"
OPENAI_API_KEY = "sk-proj-your_key_here"
GEMINI_API_KEY = "AIzaSy_your_key_here"
TAVILY_API_KEY = "tvly-your_key_here"
```

### Step 4 — Deploy

Click **Deploy** — your app will be live at `https://your-app.streamlit.app` within 2 minutes ✅

---

## ⚙️ How It Works

### RAG Pipeline

```
1. User uploads PDF/DOCX/TXT
        ↓
2. load_document()     — extracts raw text (PyMuPDF / python-docx)
        ↓
3. chunk_text()        — splits into 500-word overlapping windows
        ↓
4. EmbeddingFunction() — sentence-transformers encodes each chunk
        ↓
5. ChromaDB.upsert()   — stores vectors with metadata
        ↓
6. User asks a question
        ↓
7. embed_query()       — encodes the query
        ↓
8. collection.query()  — cosine similarity → top-K chunks
        ↓
9. build_rag_context() — formats chunks for LLM injection
```

### Web Search Pipeline

```
1. should_search_web() checks query for trigger keywords:
   latest / recent / FDA / approved / trial / guideline / 2024 / 2025...
        ↓
2. Tavily API called (if key set) — semantic, medical-aware search
        ↓
3. Falls back to DuckDuckGo if Tavily unavailable (free, always works)
        ↓
4. format_search_results() formats top-4 results for LLM injection
```

### Response Mode System

| Mode | Behaviour | Prompt Injection |
|---|---|---|
| ⚡ Concise | 2–4 sentences max | `"Be direct. Skip examples. Max 4 sentences."` |
| 📋 Detailed | Full markdown with headers & bullets | `"Comprehensive, structured, use markdown."` |
| 🧒 ELI5 | Zero jargon, everyday analogies | `"Explain as if speaking to a 12-year-old."` |

---

## 🖼️ Screenshots

### Main Chat Interface
> Beautiful dark teal theme — custom Syne + DM Sans typography

### RAG Document Upload
> Sidebar document uploader → ChromaDB indexing with chunk count

### Emergency Detection
> Red banner with Indian helpline numbers fires automatically

### Session Analytics Dashboard
> Live query categories, RAG hit rate, web search counter

---

## 🧩 Challenges & Solutions

| Challenge | Solution |
|---|---|
| Streaming tokens across 3 different LLM providers | Unified generator pattern — each provider adapter `yield`s tokens; `app.py` appends to a Streamlit `st.empty()` placeholder |
| ChromaDB cold start latency on Streamlit Cloud | Lazy collection initialisation — created once in `st.session_state`, reused across all queries |
| Web search adding latency to every query | Keyword heuristic (`should_search_web()`) gates the call — only fires when query contains recency signals |
| Context window overflow with RAG + web context combined | Hard character limits per context block (3500 chars RAG, 700 chars/result web) + `MAX_HISTORY` message trimming |
| API keys leaking into GitHub | All keys via `os.getenv()`, `.env` in `.gitignore`, Streamlit Secrets for cloud deployment |

---

## ⚠️ Medical Disclaimer

> MediBot is designed for **educational and informational purposes only**.
> It does **not** provide medical diagnoses and is **not** a substitute for professional healthcare advice.
> Always consult a licensed physician or qualified healthcare professional for personal medical decisions.
> In a medical emergency, call **112** immediately.

---

## 👤 Author

**[Your Name]**
- GitHub: [@your_username](https://github.com/your_username)
- LinkedIn: [your-linkedin](https://linkedin.com/in/your-linkedin)
- Email: your.email@example.com

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ for the NeoStats AI Engineer Case Study**

*MediBot v2.0.0 — Where technology meets healthcare intelligence*

⭐ **If this project helped you, please give it a star!** ⭐

</div>
