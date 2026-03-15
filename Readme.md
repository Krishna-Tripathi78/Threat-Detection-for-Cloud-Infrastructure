# AI-Driven Threat Detection for Cloud Infrastructure

An automated security monitoring system that analyzes AWS CloudTrail logs and detects suspicious behavior using machine learning, with AI-generated threat explanations.

---

## Overview

Cloud environments generate massive volumes of logs every minute, making manual monitoring impractical. This system continuously ingests AWS CloudTrail logs, identifies anomalous patterns using ML (Isolation Forest), and alerts administrators with AI-generated explanations of detected threats via a real-time Flask dashboard.

---

## Features

- Ingests and parses logs from AWS CloudTrail
- Detects anomalies using Isolation Forest (unusual logins, abnormal API calls, privilege escalation)
- Generates human-readable threat explanations via OpenAI API
- Triggers alerts and automated responses for suspicious activity
- Real-time Flask dashboard for logs, anomalies, and threat alerts

---

## Tech Stack

| Layer       | Technology                        |
|-------------|-----------------------------------|
| Cloud Logs  | AWS CloudTrail + boto3            |
| ML Model    | scikit-learn (Isolation Forest)   |
| AI Explainer| OpenAI API (GPT)                  |
| Alerts      | Custom alert manager + responder  |
| Dashboard   | Flask                             |
| Config      | python-dotenv                     |

---

## Project Structure

```
Ai Threat detection/
├── main.py                        # Entry point — orchestrates the pipeline
├── requirements.txt               # Python dependencies
├── .env.example                   # Environment variable template
├── .gitignore
│
├── ingestion/                     # Log collection & parsing
│   ├── __init__.py
│   ├── cloudtrail_collector.py    # Fetches logs from AWS CloudTrail
│   └── log_parser.py              # Normalizes raw log events
│
├── detection/                     # ML-based anomaly detection
│   ├── __init__.py
│   ├── model_trainer.py           # Trains Isolation Forest model
│   └── anomaly_detector.py        # Scores incoming logs for anomalies
│
├── explainer/                     # AI threat explanation
│   ├── __init__.py
│   └── threat_explainer.py        # Calls OpenAI API to explain anomalies
│
├── alerts/                        # Alerting & automated response
│   ├── __init__.py
│   ├── alert_manager.py           # Manages and dispatches alerts
│   └── responder.py               # Automated response actions
│
├── dashboard/                     # Real-time web dashboard
│   ├── __init__.py
│   └── app.py                     # Flask app — displays logs, anomalies, alerts
│
├── config/                        # Configuration
│   └── settings.py                # Loads env vars and app settings
│
└── tests/                         # Unit tests
    └── __init__.py
```

---

## Setup

1. Clone the repo and navigate into the project folder.

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Copy `.env.example` to `.env` and fill in your credentials:
   ```bash
   cp .env.example .env
   ```

   ```env
   OPENAI_API_KEY=your_openai_api_key
   AWS_ACCESS_KEY_ID=your_aws_access_key
   AWS_SECRET_ACCESS_KEY=your_aws_secret_key
   AWS_REGION=us-east-1
   ```

4. Run the system:
   ```bash
   python main.py
   ```

5. Launch the dashboard:
   ```bash
   python dashboard/app.py
   ```

---

## Pipeline Flow

```
CloudTrail Logs
      │
      ▼
 ingestion/          ← Collect & parse raw AWS events
      │
      ▼
 detection/          ← Isolation Forest scores each event
      │
      ▼
 explainer/          ← OpenAI generates threat explanation
      │
      ▼
 alerts/             ← Alert dispatched + automated response triggered
      │
      ▼
 dashboard/          ← Real-time view of all activity
```

---

## Environment Variables

| Variable                | Description                        |
|-------------------------|------------------------------------|
| `OPENAI_API_KEY`        | OpenAI API key for GPT explainer   |
| `AWS_ACCESS_KEY_ID`     | AWS access key                     |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key                     |
| `AWS_REGION`            | AWS region (e.g. `us-east-1`)      |

---

## Security Notes

- Never commit `.env` to version control — it is listed in `.gitignore`
- Use IAM roles with least-privilege permissions for CloudTrail access
- Rotate AWS credentials regularly
