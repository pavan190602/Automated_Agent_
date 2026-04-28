# 🤖 AutoAgent — Autonomous ML Engineering System

[![Python](https://img.shields.io/badge/Python-3.10+-blue)](https://python.org)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.2+-purple)](https://github.com/langchain-ai/langgraph)
[![Claude](https://img.shields.io/badge/Claude-Anthropic-orange)](https://anthropic.com)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> Give it a dataset and a problem. AutoAgent does the entire ML engineering job — EDA, feature engineering, model selection, evaluation, and explanation — with a conversational AI interface at every step.

---

## 🎯 What It Does

Most ML projects spend 70% of time on data prep and model iteration. AutoAgent automates that with a multi-agent system orchestrated by LangGraph, narrated by Claude, and explained with SHAP.

| Step | Agent | What It Does |
|------|-------|-------------|
| 1 | **EDA Agent** | Profiles dataset — stats, missing values, correlations, outliers, class balance. Claude narrates key findings. |
| 2 | **Feature Engineer** | Handles missing data, encodes categoricals, scales numerics, removes low-variance features. Claude explains every decision. |
| 3 | **Model Selector** | Trains 5 models (LR, RF, XGBoost, LightGBM, GBM), cross-validates, picks the best, explains with SHAP. Claude gives recommendations. |

---

## 🏗️ Architecture

```
React/Streamlit UI
        │
   FastAPI Gateway
        │
  LangGraph Orchestrator
  ┌─────┴──────────────────┐
  │  EDA Agent             │
  │  Feature Engineer      │  ←→  Vector DB (RAG knowledge)
  │  Model Selector        │
  └────────────────────────┘
        │
  Claude API (claude-sonnet-4)
        │
  sklearn · XGBoost · SHAP
```

---

## 🚀 Quick Start

### 1. Clone & install

```bash
git clone https://github.com/pavan190602/autoagent.git
cd autoagent
pip install -r requirements.txt
```

### 2. Set API key

```bash
cp .env.example .env
# Add your Anthropic API key to .env
ANTHROPIC_API_KEY=sk-ant-...
```

### 3. Run Streamlit UI

```bash
streamlit run streamlit_app.py
```

### 4. Or run FastAPI backend

```bash
python api.py
# Visit http://localhost:8000/docs
```

---

## 🧪 Test with Sample Data

```bash
python sample_data/generate.py   # creates sample_data/churn.csv
streamlit run streamlit_app.py
# Upload churn.csv, target=churn, task=classification
```

---

## 📦 Tech Stack

| Layer | Technology |
|-------|-----------|
| **AI / Agents** | Claude API (Anthropic), LangGraph, Tool use, RAG |
| **ML Core** | scikit-learn, XGBoost, LightGBM, SHAP, Optuna |
| **Backend** | FastAPI, Uvicorn, Pydantic |
| **Frontend** | Streamlit, Plotly |
| **Data** | Pandas, NumPy |

---

## 🔌 API Usage

```bash
curl -X POST http://localhost:8000/run \
  -F "file=@sample_data/churn.csv" \
  -F "problem_description=Predict customer churn" \
  -F "target_column=churn" \
  -F "task_type=classification"
```

Response:
```json
{
  "success": true,
  "eda_report": { "basic_stats": {...}, "claude_insight": "..." },
  "feature_report": { "encoding": [...], "claude_insight": "..." },
  "model_report": { "best_model": "XGBoost", "shap_importance": {...}, "claude_insight": "..." },
  "messages": [...]
}
```

---

## 🧠 How Claude Is Used

AutoAgent uses Claude's **tool_use** API — not simple prompting. Each agent:

1. Gathers raw data (pandas stats, sklearn metrics)
2. Defines a tool that exposes that data to Claude
3. Runs an agentic loop where Claude calls the tool, receives results, and produces a structured narrative
4. Returns both structured data (for the UI) and natural language insight (for the user)

This means Claude actually *reads* the data rather than hallucinating generic advice.

---

## 📁 Project Structure

```
autoagent/
├── orchestrator.py          # LangGraph StateGraph — the core pipeline
├── streamlit_app.py         # Full Streamlit UI
├── api.py                   # FastAPI REST endpoints
├── agents/
│   ├── eda_agent.py         # EDA + Claude insight
│   ├── feature_agent.py     # Feature engineering + Claude explanation
│   └── model_agent.py       # Model training + SHAP + Claude recommendation
├── sample_data/
│   ├── generate.py          # Sample dataset generator
│   └── churn.csv            # Generated churn dataset
├── requirements.txt
└── .env.example
```

---

## 🗺️ Roadmap

- [ ] Optimizer Agent (Optuna HPO)
- [ ] Deploy Agent (auto-generate FastAPI endpoint + Dockerfile)
- [ ] Monitor Agent (drift detection + retrain trigger)
- [ ] RAG knowledge base (indexed ML best practices)
- [ ] MLflow experiment tracking
- [ ] Docker compose setup

---

## 👤 Author

**Pavan** · MS Computer Science, University of Central Missouri  
Specialising in LLMs, RAG systems, and agentic AI  
[GitHub](https://github.com/pavan190602) · [LinkedIn](https://linkedin.com/in/pavan)

---

## 📄 License

MIT — use freely, attribution appreciated.
