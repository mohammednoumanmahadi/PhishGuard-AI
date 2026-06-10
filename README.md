<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&duration=3000&pause=1000&color=00D4FF&center=true&vCenter=true&width=600&lines=PhishGuard+AI;AI-Powered+Phishing+Detection;Built+for+SOC+Analysts" alt="Typing SVG" />

<br/>

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Ollama](https://img.shields.io/badge/Ollama-Mistral-black?style=for-the-badge&logo=ollama&logoColor=white)
![VirusTotal](https://img.shields.io/badge/VirusTotal-394EFF?style=for-the-badge&logo=virustotal&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active%20Development-orange?style=for-the-badge)

<br/>

> **A local AI-powered phishing detection system built for SOC Analysts — combining threat intelligence, email forensics, and LLM-generated SOC reports in one unified platform.**

<br/>

[Features](#-features) • [Architecture](#-architecture) • [Tech Stack](#-tech-stack) • [Installation](#-installation) • [Usage](#-usage) • [API Reference](#-api-reference)

</div>

---

## 🎯 What is PhishGuard AI?

PhishGuard AI is a **local, privacy-first phishing analysis platform** designed for security operations teams. Upload a suspicious `.eml` file and get a full threat assessment in seconds — IP reputation, domain analysis, URL scanning, attachment hashing, SPF/DKIM/DMARC validation, risk scoring, and a **Mistral-powered SOC report** — all from your own machine.

No cloud dependency. No data leakage. Just signal.

---

## ✨ Features

| Feature | Description |
|---|---|
| 📧 **Email (.eml) Forensics** | Extracts headers, sender IP, links, attachments, SPF/DKIM/DMARC |
| 🌐 **Threat Intelligence** | VirusTotal, AbuseIPDB, IPInfo lookups with local SQLite caching |
| 🔍 **WHOIS Lookup** | Domain registration intelligence for sender domain |
| ⚖️ **Risk Scoring Engine** | Weighted scoring across IP, domain, URL, auth failures |
| 🤖 **LLM Report Generation** | Mistral (via Ollama) generates professional SOC-style narratives |
| 📄 **PDF Export** | Download full phishing investigation reports as PDF |
| 🗂️ **IOC Tracker** | Aggregated view of all malicious IPs, URLs, and file hashes |
| 📊 **Analytics Dashboard** | Historical scan trends and threat statistics |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────┐
│                   React Frontend                     │
│      Dashboard │ Scanner │ History │ IOC Tracker     │
└──────────────────────┬──────────────────────────────┘
                       │ HTTP (port 5173 → 8000)
┌──────────────────────▼──────────────────────────────┐
│               FastAPI Backend (app.py)               │
└──┬───────────┬──────────────┬───────────────────────┘
   │           │              │
   ▼           ▼              ▼
Email       Threat         Database
Analyzer    Intel          (SQLite)
   │        ├─ VirusTotal      │
   │        ├─ AbuseIPDB       ▼
   │        └─ WHOIS       Cached IOCs
   │
   ▼
Risk Engine → SOC Formatter → Mistral LLM → PDF Report
```

---

## 🧰 Tech Stack

**Backend**
- `Python` + `FastAPI` + `Uvicorn`
- `python-dotenv` for API key management
- `SQLite` for scan history and IOC caching
- `Ollama / Mistral` for local LLM report generation
- `ReportLab` for PDF generation

**Frontend**
- `React 19` + `Vite`
- `Tailwind CSS`
- `Recharts` for analytics visualizations
- `React Router` for page navigation
- `Axios` for API communication

**Threat Intelligence**
- [VirusTotal API](https://www.virustotal.com) — URL, domain & file hash scanning
- [AbuseIPDB API](https://www.abuseipdb.com) — IP reputation
- [IPInfo API](https://ipinfo.io) — Geolocation & ASN data

---

## 📁 Project Structure

```
PhishGuard-AI/
├── core/
│   ├── email_analyzer.py      # .eml parsing, header extraction
│   ├── threat_intel.py        # VirusTotal, AbuseIPDB, IPInfo
│   ├── whois_lookup.py        # Domain WHOIS
│   ├── risk_engine.py         # Weighted risk scoring
│   ├── soc_formatter.py       # Structured SOC output
│   ├── llm_report.py          # Mistral report generation
│   ├── pdf_report.py          # PDF export
│   ├── database.py            # SQLite persistence & caching
│   ├── pipeline.py            # Full scan orchestration
│   └── api.env.example        # API key template
│
├── gui/
│   ├── app.py                 # FastAPI entrypoint (8 endpoints)
│   └── frontend/
│       └── src/pages/
│           ├── Dashboard.jsx  # Overview & stats
│           ├── Scanner.jsx    # Upload & scan
│           ├── History.jsx    # Past scans
│           ├── IOCTracker.jsx # Malicious IOC aggregation
│           └── Analytics.jsx  # Charts & trends
│
├── scripts/                   # Unit tests for each module
└── README.md
```

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/mohammednoumanmahadi/PhishGuard-AI.git
cd PhishGuard-AI
```

### 2. Set up API keys

```bash
cp core/api.env.example core/api.env
```

Edit `core/api.env`:

```env
VT_API_KEY=your_virustotal_api_key
ABUSEIPDB_API_KEY=your_abuseipdb_api_key
IPINFO_API_KEY=your_ipinfo_api_key
```

### 3. Install Python dependencies

```bash
pip install fastapi uvicorn python-dotenv requests python-whois reportlab
```

### 4. Install and start Ollama (Mistral)

```bash
# Install from https://ollama.com
ollama pull mistral
ollama serve
```

### 5. Start the backend

```bash
cd gui
uvicorn app:app --reload --port 8000
```

### 6. Start the frontend

```bash
cd gui/frontend
npm install
npm run dev
```

The app will be running at **http://localhost:5173**

---

## 🚀 Usage

1. Open the app at `http://localhost:5173`
2. Navigate to the **Scanner** page
3. Upload a suspicious `.eml` file
4. Review the full analysis:
   - Risk score and severity classification
   - IP, domain, and URL threat intel results
   - SPF / DKIM / DMARC authentication status
   - WHOIS domain registration data
5. Generate an **LLM-powered SOC report**
6. Export as **PDF** for incident documentation

---

## 📡 API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/health` | Health check |
| `GET` | `/api/stats` | Dashboard statistics |
| `GET` | `/api/scans` | All historical scans |
| `GET` | `/api/scan/{id}` | Single scan detail |
| `POST` | `/api/scan` | Upload & analyze `.eml` file |
| `POST` | `/api/report/{id}` | Generate LLM SOC report |
| `GET` | `/api/pdf/{id}` | Download PDF report |
| `GET` | `/api/iocs` | Aggregated IOC tracker |

---

## 🗺️ Roadmap

- [ ] YARA rule integration for attachment analysis
- [ ] MITRE ATT&CK technique tagging
- [ ] Slack/Teams alert webhook support
- [ ] Multi-email batch processing
- [ ] Threat actor correlation across scans
- [ ] STIX/TAXII export for SIEM ingestion

---

## 👤 Author

**Mohammed Nouman**  
SOC Analyst & AI Security Engineer | Abu Dhabi, UAE

[![GitHub](https://img.shields.io/badge/GitHub-mohammednoumanmahadi-181717?style=flat-square&logo=github)](https://github.com/mohammednoumanmahadi)

---

## ⚠️ Disclaimer

This tool is built for **defensive security research and SOC operations only**. Always ensure you have proper authorization before analyzing any email. API keys must be obtained from respective providers under their terms of service.

---

<div align="center">

**⭐ If PhishGuard AI helped your SOC workflow, please star this repo!**

</div>
