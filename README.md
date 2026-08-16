# ⚙️ Agentic AI for Smart Facility Operations and Optimization

A Streamlit-based enterprise dashboard for predictive maintenance, powered by a locally-hosted LLM (via [Ollama](https://ollama.com)). The system ingests industrial sensor telemetry, detects machine failures, generates AI-driven diagnostic reports, and manages the full maintenance lifecycle — from work order creation to preventive maintenance scheduling.

---

## 📌 Overview

This application simulates a real-world **Computerized Maintenance Management System (CMMS)** enhanced with agentic AI capabilities. It uses the **AI4I 2020 Predictive Maintenance Dataset** to monitor machine health, automatically flags failures, and triggers a local LLM to generate structured maintenance recommendations — all without relying on any external API (fully offline-capable via Ollama).

Key highlights:
- 🔐 Role-based authentication (Admin / Technician)
- 📊 Real-time KPI dashboard with interactive Plotly visualizations
- 🔍 Individual machine telemetry explorer with auto-redirect on failure detection
- 🤖 AI Maintenance Assistant that generates structured diagnostic reports using a local LLM
- 🔨 Work order creation, tracking, and lifecycle management
- 🗓️ Preventive Maintenance (PM) scheduler with AI-recommended maintenance frequencies
- 📄 Exportable PDF and Word (.docx) maintenance reports
- 🎨 Custom high-contrast "Deep Space" enterprise UI theme

---

## 🖥️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend / App Framework | [Streamlit](https://streamlit.io) |
| Data Handling | Pandas, NumPy |
| Visualization | Plotly (Express & Graph Objects), Matplotlib, Seaborn |
| Database | SQLite3 (local, file-based) |
| AI / LLM | [Ollama](https://ollama.com) (local inference, e.g. `llama3.2:1b`) |
| Document Generation | WeasyPrint / fpdf2 (PDF), python-docx (Word) |
| Auth | SHA-256 hashed credentials stored in SQLite |

---

## 📂 Project Structure

```
.
├── app.py              # Main Streamlit application (all logic & UI)
├── ai4i2020.csv         # Required dataset (not included — see below)
├── work_orders.db        # Auto-generated SQLite database (created on first run)
└── README.md
```

---

## ⚙️ Prerequisites

1. **Python 3.9+**
2. **Ollama** installed and running locally for AI features:
   ```bash
   # Install Ollama: https://ollama.com/download
   ollama serve
   ollama pull llama3.2:1b
   ```
3. **The AI4I 2020 dataset** (`ai4i2020.csv`), placed in the same directory as `app.py`. It can be downloaded from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/601/ai4i+2020+predictive+maintenance+dataset).

---

## 📦 Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. (Recommended) create a virtual environment
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install streamlit pandas numpy plotly matplotlib seaborn requests weasyprint fpdf2 python-docx

# 4. Place the dataset
# Download ai4i2020.csv and place it in the project root folder

# 5. Run the app
streamlit run app.py
```

The app will open in your browser at `http://localhost:8501`.

> 💡 If WeasyPrint fails to install/run on your system (it has native OS dependencies), the app automatically falls back to `fpdf2` for PDF generation — no action needed.

---

## 🔑 Demo Credentials

| Role | Username | Password |
|---|---|---|
| Admin | `admin` | `admin123` |
| Technician | `david` | `tech123` |

> ⚠️ These are demo credentials seeded automatically into the local SQLite database on first run. **Change or remove them before any production deployment.**

---

## 🧭 Application Modules

| Module | Access | Description |
|---|---|---|
| **Dashboard** | All roles | Fleet-wide KPIs, failure mode distribution, tool wear analysis, RPM vs Torque correlation |
| **Data Analysis (EDA)** | Admin only | Dataset preview, statistical summary, feature correlation heatmap |
| **Machine Explorer** | All roles | Search/select individual machines, view live telemetry, auto-redirects Admins to AI Assistant on failure detection |
| **AI Maintenance Assistant** | Admin only | Generates a structured diagnostic report via a local LLM (Ollama) based on machine telemetry |
| **Work Order Creation** | Admin only | Create and store maintenance work orders, optionally attaching the AI-generated report |
| **Work Order Management** | All roles (filtered by technician for non-admins) | Search, filter, update status, delete, and export work orders as PDF/DOCX |
| **Preventive Maintenance Scheduler** | Admin only | Create recurring PM schedules, track overdue/upcoming tasks, and get AI-recommended maintenance frequencies |

---

## 🤖 How the AI Integration Works

- The app sends a structured prompt to a **local Ollama server** (`http://localhost:11434/api/generate`) — no data leaves your machine.
- The default model is `llama3.2:1b`, but this can be changed in the UI to any locally pulled Ollama model.
- The **AI Maintenance Assistant** returns a plain-text structured report (Executive Summary, KPIs, Root Cause, Recommendations, Maintenance Schedule).
- The **AI Recommendations** tab (under PM Scheduler) requests strict JSON output to auto-populate recommended maintenance frequency and checklists.
- If Ollama is not running, the app displays clear connection-error messages and gracefully continues without blocking other features.

---

## 🔒 Security Notes

- Passwords are stored as **SHA-256 hashes**, never in plaintext.
- The SQLite database (`work_orders.db`) is generated locally and is **not included** in this repository — do not commit it, as it may contain hashed credentials and operational data.
- No external API keys are required; all AI inference is local via Ollama.

---

## 📄 License

This project is provided for educational/academic purposes. Add your preferred license here (e.g., MIT).

---

## 🙋 Support

For issues or questions, please open an issue in this repository.
