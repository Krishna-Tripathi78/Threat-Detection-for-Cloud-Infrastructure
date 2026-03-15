# 8-Week Roadmap — AI-Driven Threat Detection

**2 months · 30 minutes/day · 56 days**

Keep `daily_log.md` updated every day with:
1. What I learned
2. What I built
3. What failed / next step

---

## Week 1 — Security + Cloud Logging Foundations (Days 1–7)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 1 | Threat detection lifecycle in cloud | Create repo + folders + `daily_log.md` | Base project scaffold ready |
| 2 | AWS IAM basics (users, roles, policies) | Write one sample least-privilege IAM policy JSON | `notes/iam_policy_sample.json` |
| 3 | CloudTrail event structure | Download/read 5 sample CloudTrail events | `notes/cloudtrail_fields.md` |
| 4 | Types of attacks in logs (brute force, privilege escalation) | Map 5 attacks to related CloudTrail fields | `notes/attack_to_fields.md` |
| 5 | JSON log parsing in Python | Parse sample log JSON and print key fields | `ingestion/log_parser.py` started |
| 6 | Basic detection strategy design | Define "normal vs suspicious" for your project | `notes/detection_rules_v1.md` |
| 7 | Weekly review | Clean repo, update README progress section | Week 1 complete checklist |

---

## Week 2 — Data Pipeline Setup (Days 8–14)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 8 | Pandas basics for log analysis | Load logs into DataFrame | `ingestion/log_parser.py` loads CSV |
| 9 | Timestamp handling | Convert event times to UTC datetime | `event_time` column normalized |
| 10 | Missing data handling | Add fill/drop strategy for null values | Null handling in `log_parser.py` |
| 11 | Feature engineering basics | Create `is_console_login`, `is_failed_auth` features | Feature columns added to DataFrame |
| 12 | IP and geo concepts | Add IP extraction + placeholder geo field | `ingestion/log_parser.py` IP fields |
| 13 | Dataset splitting (train/test) | Save train/test CSV from processed logs | `data/train.csv`, `data/test.csv` |
| 14 | Weekly review | Add mini pipeline script that runs all preprocess steps | `main.py` preprocess section done |

---

## Week 3 — ML Anomaly Detection Core (Days 15–21)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 15 | Isolation Forest concept | Train first Isolation Forest model | `detection/model_trainer.py` started |
| 16 | Anomaly score interpretation | Generate anomaly scores on test set | `anomaly_scores.csv` in `data/` |
| 17 | Threshold tuning | Set initial threshold for suspicious events | `notes/threshold_config.md` |
| 18 | False positives vs false negatives | Label 20 events manually for sanity check | `notes/manual_validation.md` |
| 19 | Feature impact basics | Print top features used in anomaly input | `notes/feature_summary.md` |
| 20 | Model persistence | Save model with joblib | `models/isolation_forest.joblib` |
| 21 | Weekly review | One script: preprocess + predict | `detection/anomaly_detector.py` done |

---

## Week 4 — Threat Intelligence Layer (Days 22–28)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 22 | Rule-based + ML hybrid detection | Add 3 hard rules (root login, key misuse, impossible travel placeholder) | `detection/rules_engine.py` |
| 23 | MITRE ATT&CK basics | Map 5 suspicious patterns to ATT&CK tactics | `notes/mitre_mapping.md` |
| 24 | Risk scoring | Create simple risk score formula (rule + anomaly score) | `detection/risk_score.py` |
| 25 | Alert schema design | Define fields: severity, source, event, reason | `notes/alert_schema.json` |
| 26 | Alert deduplication | Add logic to merge repeated alerts | `alerts/alert_manager.py` dedup logic |
| 27 | Severity classification | Tag alerts as Low / Medium / High / Critical | Severity field in alert output |
| 28 | Weekly review | Export final alert list from sample logs | `samples/alerts_output.json` |

---

## Week 5 — AI Threat Explanation (Days 29–35)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 29 | Prompt engineering for security summaries | Write prompt template for threat explanation | `notes/threat_explain_prompt.txt` |
| 30 | OpenAI API integration basics | Build simple call wrapper for explanation | `explainer/threat_explainer.py` started |
| 31 | Structured AI output | Request JSON response format (summary, impact, action) | Parsed AI output object |
| 32 | Guardrails for AI responses | Add max tokens + timeout + fallback text | Safe API wrapper in `threat_explainer.py` |
| 33 | Explain sample alerts | Generate explanation for 3 suspicious events | `samples/ai_explanations.json` |
| 34 | Prompt quality improvement | Reduce hallucination by grounding with raw event fields | Improved prompt v2 in notes |
| 35 | Weekly review | Integrate AI explanation into alert pipeline | `samples/enriched_alerts.json` |

---

## Week 6 — Automated Response + API (Days 36–42)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 36 | Incident response basics | Define response playbook per severity | `notes/response_playbook.md` |
| 37 | Safe automation principles | Add `simulate_mode` flag for all responses | `config/settings.py` simulate flag |
| 38 | Blocklist workflow (simulation) | Build function to add suspicious IP to local blocklist | `alerts/responder.py` + `samples/blocked_ips.json` |
| 39 | Credential compromise handling | Build function to mark key/user for disable action (simulated) | `samples/response_actions.json` |
| 40 | Flask basics | Create Flask app with `/api/alerts` endpoint | `dashboard/app.py` started |
| 41 | API endpoint for responses | Create `/api/respond` to trigger simulated action | Response action endpoint working |
| 42 | Weekly review | Test API with curl/Postman and document routes | `notes/api_test_notes.md` |

---

## Week 7 — Dashboard + Monitoring (Days 43–49)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 43 | Dashboard design for SOC-style view | Draft dashboard layout (cards + alert table + trend chart) | `notes/dashboard_wireframe.md` |
| 44 | Jinja2 templating basics | Build `base.html` + `index.html` summary cards | `dashboard/templates/` page 1 |
| 45 | Time-series chart with Chart.js | Add alerts-over-time line chart | Trend chart visible in browser |
| 46 | Severity visualization | Add severity distribution bar/pie chart | Severity chart on dashboard |
| 47 | Alert detail panel | Show full event + AI explanation on click | Alert detail view in `alerts.html` |
| 48 | Basic filters | Add filters by date / severity / service | Working filter controls |
| 49 | Weekly review | Connect dashboard to Flask API endpoints | Live dashboard + API integrated |

---

## Week 8 — Hardening + Multi-Cloud Overview + Final Packaging (Days 50–56)

| Day | Learn (10m) | Tiny Task (15m) | Expected Output |
|-----|-------------|-----------------|-----------------|
| 50 | Logging and observability for your app | Add Python `logging` for all pipeline steps | `app_runtime.log` generated |
| 51 | Basic unit testing strategy | Add 2 tests (parser + risk scoring) | `tests/unit/test_processing.py` |
| 52 | Config management and secrets | Move all configurable values to `.env` + `config/settings.py` | `.env.example` updated |
| 53 | Multi-cloud concept mapping | Create AWS → Azure / GCP event field mapping table | `notes/multi_cloud_mapping.md` |
| 54 | Azure and GCP audit log basics | Add parser placeholders for Azure / GCP schema | `ingestion/log_parser.py` stubs |
| 55 | Final integration run | Execute end-to-end: ingest → detect → explain → respond → dashboard | Final demo output in `samples/` |
| 56 | Demo + documentation day | Record final walkthrough notes and update README | Project v1 complete ✅ |

---

## Summary

| Week | Focus                              | Deliverable                         |
|------|------------------------------------|-------------------------------------|
| 1    | Security + cloud logging basics    | Understand logs and attack patterns |
| 2    | Data pipeline setup                | Working preprocess pipeline         |
| 3    | ML anomaly detection               | Trained anomaly model               |
| 4    | Threat intelligence layer          | Risk-scored alert output            |
| 5    | AI threat explanation              | AI explanations integrated          |
| 6    | Automated response + API           | Response simulation + Flask API     |
| 7    | Dashboard + monitoring             | Working real-time dashboard         |
| 8    | Hardening + multi-cloud + docs     | End-to-end demo + project complete  |

---

## Skills You Will Learn

- AWS (IAM, CloudTrail, boto3)
- Machine Learning (Isolation Forest, feature engineering, threshold tuning)
- OpenAI API (prompt engineering, structured output, guardrails)
- Python (pandas, logging, packaging, pytest + mocks)
- Flask (routing, Jinja2 templates, REST API design)
- JavaScript (fetch API, Chart.js, DOM manipulation)
- Security best practices (least privilege, secrets management, MITRE ATT&CK)
- Multi-cloud awareness (AWS, Azure, GCP log schema differences)

---

## Lightweight Learning Sources (10 min/day only)

- AWS CloudTrail docs — event reference
- scikit-learn anomaly detection docs
- Flask official tutorial
- OpenAI API docs (chat/completions)
- Chart.js quickstart

---

> Focus on **consistency over intensity**. 30 minutes daily for 56 days builds a solid working prototype if you always produce one tiny output each day.
