<div align="center">

# 🛡️ AI-Driven Threat Detection for Cloud Infrastructure

**An intelligent security monitoring system that ingests AWS CloudTrail logs, detects anomalies using machine learning, and delivers AI-generated threat explanations through a real-time dashboard.**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.x-black?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-Isolation%20Forest-orange?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT-412991?logo=openai&logoColor=white)](https://platform.openai.com/)
[![AWS](https://img.shields.io/badge/AWS-CloudTrail%20%7C%20IAM%20%7C%20EC2-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

[Features](#features) · [Architecture](#architecture) · [Quick Start](#quick-start) · [Project Structure](#project-structure) · [Roadmap](#roadmap) · [Contributing](#contributing)

</div>

---

## Overview

Cloud environments generate millions of log events every day, making manual security monitoring impossible at scale. This system automates the full threat detection lifecycle:

1. **Ingests** raw AWS CloudTrail events via `boto3`
2. **Parses & engineers** security-relevant features from log data
3. **Detects** anomalous behavior using an Isolation Forest ML model
4. **Explains** threats in plain English using the OpenAI API
5. **Responds** automatically with severity-based actions (alert, blocklist, IAM revocation)
6. **Visualizes** all activity on a real-time Flask dashboard

---

## Features

- 🔍 **Log Ingestion** — Pulls live CloudTrail events or processes sample logs offline
- 🤖 **ML Anomaly Detection** — Isolation Forest scores every event; tunable contamination threshold
- 🧠 **AI Threat Explanation** — GPT generates human-readable summaries with impact and recommended action
- 🚨 **Severity-Based Alerting** — Classifies threats as `LOW` / `MEDIUM` / `HIGH` / `CRITICAL`
- ⚡ **Automated Response** — Simulated IP blocklist, IAM session revocation, and SNS notifications
- 📊 **Real-Time Dashboard** — Flask + Chart.js showing live alerts, anomaly trends, and severity distribution
- 🌐 **Multi-Cloud Ready** — Parser stubs for Azure Monitor and GCP Cloud Audit Logs

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Pipeline Flow                        │
└─────────────────────────────────────────────────────────────┘

  AWS CloudTrail
       │
       ▼
  ┌──────────┐
  │ingestion │  cloudtrail_collector.py → log_parser.py
  └──────────┘  Fetches events · Normalizes fields · Engineers features
       │
       ▼
  ┌──────────┐
  │detection │  model_trainer.py → anomaly_detector.py → rules_engine.py
  └──────────┘  Isolation Forest · Risk scoring · Hard rules (MITRE ATT&CK)
       │
       ▼
  ┌──────────┐
  │explainer │  threat_explainer.py
  └──────────┘  OpenAI GPT → summary · impact · recommended action
       │
       ▼
  ┌──────────┐
  │  alerts  │  alert_manager.py → responder.py
  └──────────┘  Severity tagging · Deduplication · Automated response
       │
       ▼
  ┌──────────┐
  │dashboard │  Flask + Jinja2 + Chart.js
  └──────────┘  Real-time alerts · Anomaly trends · Severity charts
```

---

## Tech Stack

| Layer            | Technology                          |
|------------------|-------------------------------------|
| Cloud Logs       | AWS CloudTrail + `boto3`            |
| ML Model         | `scikit-learn` — Isolation Forest   |
| AI Explainer     | OpenAI API (GPT-4o)                 |
| Alerting         | Custom alert manager + responder    |
| Dashboard        | Flask + Jinja2 + Chart.js           |
| Configuration    | `python-dotenv`                     |
| Testing          | `pytest` + `unittest.mock`          |

---

## Project Structure

```
Ai Threat detection/
├── main.py                          # Pipeline entry point
├── requirements.txt                 # Python dependencies
├── .env.example                     # Environment variable template
├── .gitignore
├── daily_log.md                     # Day-by-day progress log
│
├── ingestion/                       # Log collection & parsing
│   ├── cloudtrail_collector.py      # Fetches events from AWS CloudTrail
│   └── log_parser.py                # Normalizes & engineers features
│
├── detection/                       # Anomaly detection
│   ├── model_trainer.py             # Trains & saves Isolation Forest
│   └── anomaly_detector.py          # Scores events, applies thresholds
│
├── explainer/                       # AI threat explanation
│   └── threat_explainer.py          # OpenAI prompt builder & response parser
│
├── alerts/                          # Alerting & response
│   ├── alert_manager.py             # Creates, deduplicates & dispatches alerts
│   └── responder.py                 # Automated response actions
│
├── dashboard/                       # Web dashboard
│   ├── app.py                       # Flask app & API routes
│   ├── templates/                   # Jinja2 HTML templates
│   └── static/
│       ├── css/                     # Stylesheets
│       ├── js/                      # Chart.js + fetch polling
│       └── img/                     # Icons & assets
│
├── config/
│   └── settings.py                  # Loads all env vars & app config
│
├── data/                            # Processed train/test CSVs
├── models/                          # Saved model files (.joblib)
├── notes/                           # Research notes & mappings
├── samples/                         # Sample logs, alerts, AI outputs
│
└── tests/
    ├── unit/                        # Unit tests per module
    └── integration/                 # End-to-end pipeline tests
```

---

## Quick Start

### Prerequisites

- Python 3.10+
- An AWS account with CloudTrail enabled
- An OpenAI API key

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/ai-threat-detection.git
cd ai-threat-detection

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
cp .env.example .env
# Edit .env and fill in your credentials
```

### Environment Variables

Create a `.env` file from `.env.example`:

```env
OPENAI_API_KEY=your_openai_api_key
AWS_ACCESS_KEY_ID=your_aws_access_key
AWS_SECRET_ACCESS_KEY=your_aws_secret_key
AWS_REGION=us-east-1
ANOMALY_THRESHOLD=-0.1
FLASK_PORT=5000
FLASK_DEBUG=false
```

| Variable                | Required | Description                              |
|-------------------------|----------|------------------------------------------|
| `OPENAI_API_KEY`        | ✅       | OpenAI API key for GPT explainer         |
| `AWS_ACCESS_KEY_ID`     | ✅       | AWS access key                           |
| `AWS_SECRET_ACCESS_KEY` | ✅       | AWS secret key                           |
| `AWS_REGION`            | ✅       | AWS region (e.g. `us-east-1`)            |
| `ANOMALY_THRESHOLD`     | ❌       | Isolation Forest score cutoff (default `-0.1`) |
| `FLASK_PORT`            | ❌       | Dashboard port (default `5000`)          |
| `FLASK_DEBUG`           | ❌       | Enable Flask debug mode (default `false`)|

### Run the Pipeline

```bash
# Run the full detection pipeline
python main.py

# Launch the dashboard (separate terminal)
python dashboard/app.py
```

Open [http://localhost:5000](http://localhost:5000) in your browser.

### Run Tests

```bash
pytest tests/ -v
```

---

## API Endpoints

| Method | Endpoint        | Description                              |
|--------|-----------------|------------------------------------------|
| `GET`  | `/`             | Dashboard home page                      |
| `GET`  | `/api/logs`     | Returns all parsed log events as JSON    |
| `GET`  | `/api/anomalies`| Returns anomaly-flagged events           |
| `GET`  | `/api/alerts`   | Returns all generated alerts             |
| `POST` | `/api/run`      | Triggers the full pipeline on demand     |
| `POST` | `/api/respond`  | Triggers a simulated response action     |

---

## Security Notes

> ⚠️ Never commit your `.env` file — it is excluded via `.gitignore`

- Use **IAM roles with least-privilege** permissions for CloudTrail access — never use root credentials
- All response actions run in **simulate mode** by default — set `SIMULATE_MODE=false` only in controlled environments
- Rotate AWS credentials regularly and prefer **IAM roles over long-lived access keys** where possible
- Review `notes/iam_policy_sample.json` for the minimum required IAM policy

---

## Roadmap

See [`ROADMAP.md`](ROADMAP.md) for the full 8-week, 30-minutes/day build plan.

| Week | Focus                           | Deliverable                        |
|------|---------------------------------|------------------------------------|
| 1    | Security + cloud logging basics | Understand logs and attack patterns|
| 2    | Data pipeline setup             | Working preprocess pipeline        |
| 3    | ML anomaly detection            | Trained Isolation Forest model     |
| 4    | Threat intelligence layer       | Risk-scored alert output           |
| 5    | AI threat explanation           | AI explanations integrated         |
| 6    | Automated response + API        | Response simulation + Flask API    |
| 7    | Dashboard + monitoring          | Working real-time dashboard        |
| 8    | Hardening + docs + multi-cloud  | End-to-end demo complete           |

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please ensure all new code includes unit tests and passes `pytest tests/`.

---

## License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
Built as a learning project · AWS + ML + AI + Flask · See <a href="ROADMAP.md">ROADMAP.md</a> to build it yourself
</div>
