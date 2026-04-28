# 🤖 AutoAgent — Autonomous ML Engineering System

[![Python](https://img.shields.io/badge/Python-3.10+-blue)](https://python.org)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.2+-purple)](https://github.com/langchain-ai/langgraph)
[![Claude](https://img.shields.io/badge/Claude-Anthropic-orange)](https://anthropic.com)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> **Give it a spreadsheet. Tell it what to predict. It does the entire machine learning job for you — and explains everything in plain English.**

No machine learning experience required. No coding after setup. Works with any CSV or Excel file.

---

## 🎬 What does it actually do?

Imagine you have a spreadsheet of customer records and you want to know: **"Which customers are likely to cancel?"**

Normally this takes a data scientist days of work. AutoAgent does it in minutes:

1. **Reads your data** — finds patterns, missing values, outliers, and which columns matter most
2. **Cleans and prepares it** — fills missing values, converts text to numbers, scales everything properly
3. **Tests 5 AI models** — picks the best one and shows you exactly why
4. **Explains everything** — Claude (Anthropic's AI) writes a plain-English analysis of every step

---

## 🚀 Quick Start — Get Running in 5 Minutes

### Step 1 — Make sure Python is installed

Open your terminal (Mac/Linux) or Command Prompt (Windows) and run:
```bash
python --version
```
You need Python 3.10 or higher. If you don't have it → [python.org/downloads](https://python.org/downloads)

> **Windows users:** During Python installation, check **"Add Python to PATH"**

---

### Step 2 — Download AutoAgent

```bash
git clone https://github.com/pavan190602/autoagent.git
cd autoagent
```

No Git? Click the green **"Code"** button at the top of this page → **"Download ZIP"** → unzip it → open that folder in your terminal.

---

### Step 3 — Install dependencies

```bash
pip install -r requirements.txt
```

This takes 2–5 minutes. That's normal.

---

### Step 4 — Get your free API key

AutoAgent uses Claude AI to generate explanations. You need a free Anthropic API key:

1. Go to **[console.anthropic.com](https://console.anthropic.com)**
2. Sign up (free)
3. Click **API Keys** → **Create Key**
4. Copy the key (starts with `sk-ant-...`)

Then create a `.env` file in the autoagent folder:
```bash
# Mac/Linux
echo "ANTHROPIC_API_KEY=your-key-here" > .env

# Windows
echo ANTHROPIC_API_KEY=your-key-here > .env
```

---

### Step 5 — Run it

```bash
streamlit run app.py
```

Your browser opens automatically at `http://localhost:8501`. Done.

---

## 🖥️ Using the Interface

The UI walks you through everything step by step:

| Step | What you do |
|------|------------|
| **1 — API Key** | Paste your Anthropic key (or it reads from `.env` automatically) |
| **2 — Upload Data** | Upload your CSV/Excel file, or use one of the built-in sample datasets |
| **3 — Configure** | Pick which column you want to predict, and whether it's a category or a number |
| **4 — Run** | Click the button and watch the agents work |

**No data?** Use the built-in Titanic, California Housing, or Iris datasets — perfect for trying it out.

---

## 📊 Understanding Your Results

After running, you get three tabs:

### 🔍 "What's in my data?" tab
- How many rows and columns your dataset has
- Which columns have missing values (and how much)
- Which columns are most correlated with what you're predicting
- Claude's written analysis of the key findings

### ⚙️ "How was it prepared?" tab
- Every transformation applied to your data, explained in plain English
- Why each transformation was chosen
- How many features are being used for training

### 🤖 "Which model won?" tab
- A comparison of all 5 models tested
- The winning model and its score
- **SHAP chart** — shows which columns in your data had the most influence on predictions
- Claude's full recommendation on which model to use and why

**What do the scores mean?**
- **F1 Score** (for classification — Yes/No predictions): Above 80% is very good. 100% is perfect.
- **R² Score** (for regression — number predictions): Above 0.7 is good. 1.0 is perfect.

---

## 🏗️ How It Works (Technical)

```
┌─────────────────────────────────────────────────────────────┐
│                     Streamlit UI (app.py)                    │
└──────────────────────────┬──────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│              LangGraph Orchestrator (orchestrator.py)        │
│                    State machine pipeline                    │
└──────┬──────────────────┬──────────────────┬────────────────┘
       │                  │                  │
┌──────▼──────┐  ┌────────▼──────┐  ┌───────▼────────┐
│  EDA Agent  │  │ Feature Agent │  │  Model Agent   │
│             │→ │               │→ │                │
│ Profile &   │  │ Clean &       │  │ Train, eval,   │
│ analyse     │  │ transform     │  │ SHAP explain   │
└──────┬──────┘  └───────┬───────┘  └───────┬────────┘
       │                 │                  │
       └─────────────────▼──────────────────┘
                         │
              ┌──────────▼──────────┐
              │    Claude API        │
              │  (Plain English      │
              │   explanations)      │
              └─────────────────────┘
```

**Agent breakdown:**

| Agent | Input | Output |
|-------|-------|--------|
| **EDA Agent** | Raw dataset | Stats, missing values, correlations, outliers + Claude insight |
| **Feature Agent** | Raw dataframe | Cleaned & encoded dataframe + transformation log + Claude explanation |
| **Model Agent** | Engineered features | 5 model scores, best model, SHAP importance + Claude recommendation |

---

## 📦 Tech Stack

| Layer | Technology |
|-------|-----------|
| **Agent orchestration** | [LangGraph](https://github.com/langchain-ai/langgraph) — state machine pipeline |
| **AI / LLM** | [Claude API](https://anthropic.com) (claude-sonnet-4) — tool use + explanations |
| **ML models** | scikit-learn, XGBoost, LightGBM |
| **Explainability** | SHAP (SHapley Additive exPlanations) |
| **Backend** | FastAPI + Uvicorn |
| **Frontend** | Streamlit + Plotly |
| **Data** | Pandas, NumPy |

---

## 📁 Project Structure

```
autoagent/
├── app.py                   ← Beginner-friendly UI (start here)
├── streamlit_app.py         ← Advanced UI with full technical detail
├── api.py                   ← FastAPI REST API for developers
├── orchestrator.py          ← LangGraph pipeline connecting all agents
├── agents/
│   ├── eda_agent.py         ← Agent 1: EDA + Claude insight
│   ├── feature_agent.py     ← Agent 2: Feature engineering + explanation
│   └── model_agent.py       ← Agent 3: Model training + SHAP + recommendation
├── sample_data/
│   ├── generate.py          ← Script to generate sample churn dataset
│   └── churn.csv            ← Ready-to-use sample dataset
├── requirements.txt
├── .env.example             ← Copy this to .env and add your API key
└── SETUP_GUIDE.md           ← Detailed setup guide for complete beginners
```

---

## 🔌 REST API (for developers)

Run the backend:
```bash
python api.py
# Docs at http://localhost:8000/docs
```

Run a pipeline via API:
```bash
curl -X POST http://localhost:8000/run \
  -F "file=@sample_data/churn.csv" \
  -F "problem_description=Predict customer churn" \
  -F "target_column=churn" \
  -F "task_type=classification"
```

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---------|-----|
| `ModuleNotFoundError` | `pip install -r requirements.txt` |
| `ANTHROPIC_API_KEY not found` | Check your `.env` file exists and has the key |
| Browser didn't open | Go to `http://localhost:8501` manually |
| Port 8501 in use | `streamlit run app.py --server.port 8502` |
| Pip not found (Mac) | Use `pip3` instead of `pip` |
| Permission error (Mac/Linux) | Add `--user` flag to pip install |

**Still stuck?** Open an issue → describe what you did and paste the error message.

---

## 🗺️ Roadmap

- [ ] Optimizer Agent — Optuna hyperparameter tuning
- [ ] Deploy Agent — auto-generate FastAPI endpoint + Dockerfile for your trained model
- [ ] Monitor Agent — detect data drift and trigger retraining
- [ ] RAG knowledge base — index ML literature for smarter agent decisions
- [ ] MLflow integration — experiment tracking

---

## 👤 Author

**Pavan** · MS Computer Science, University of Central Missouri  
Specialising in LLMs, RAG systems, and agentic AI  
[GitHub](https://github.com/pavan190602) · [LinkedIn](https://linkedin.com/in/pavan)

---

## 📄 License

MIT — free to use, modify, and distribute. Attribution appreciated.
