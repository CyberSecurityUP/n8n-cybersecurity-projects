# n8n-Cybersecurity-Projects

[![n8n](https://img.shields.io/badge/automation-n8n-24b47e.svg)](https://n8n.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg)](#license)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-blueviolet.svg)

A curated collection of **n8n** workflows for common **cybersecurity** tasks.
All workflows are provided as **JSON** so you can import them directly into your n8n instance (Self-hosted, Desktop, or Cloud).

---

## ✨ What’s inside

* **Ready-to-import JSON workflows** for:

  * IOC enrichment (IP, domain, hash)
  * Threat intel lookups (VirusTotal, AbuseIPDB, Shodan, MISP, HIBP, etc.)
  * Alerting to Slack/Discord/Email
  * Ticketing/Case management (Jira, GitHub Issues, TheHive)
  * Log ingestion & parsing (webhooks, files, SIEM APIs)
  * Triage & auto-response examples (with manual approval steps)
* **Reusable sub-workflows** (error handling, rate-limit backoff, pagination)
* **Environment-variable friendly** templates (no secrets in JSON)

> ⚠️ **Important:** These workflows are meant for **defensive & blue-team** automation, training, and research. Do **not** use them for unlawful activity.

---

## 📁 Repository structure

```
n8n-cybersecurity-projects/
├─ workflows/
│  ├─ enrichment/
│  │  ├─ vt_hash_enrichment.json
│  │  ├─ abuseipdb_ip_reputation.json
│  │  └─ shodan_host_footprint.json
│  ├─ alerting/
│  │  ├─ discord_alert_on_malicious_ioc.json
│  │  └─ slack_daily_threat_digest.json
│  ├─ case-management/
│  │  ├─ thehive_auto_case_from_webhook.json
│  │  └─ jira_incident_from_alert.json
│  └─ utilities/
│     ├─ http_retry_backoff_subflow.json
│     └─ json_normalize_ioc_list.json
├─ docs/
│  ├─ diagrams/ (optional PNG/SVG exports)
│  └─ usage-notes.md
├─ .env.example
└─ README.md
```

---

## ✅ Requirements

* **n8n** v1.40+ (recommended)

  * Self-hosted (Docker) or n8n Desktop/Cloud
* API keys for any services you plan to use (e.g., VirusTotal, Shodan, AbuseIPDB, HIBP, Slack/Discord webhooks)

---

## 🚀 Quick start

1. **Clone** the repository:

   ```bash
   git clone https://github.com/<your-org>/n8n-cybersecurity-projects.git
   ```

2. **Prepare environment variables**:

   * Copy `.env.example` to `.env` and fill in your keys.
   * In n8n, map these variables into **Credentials** or **Generic HTTP Request** nodes.

3. **Import a workflow** into n8n:

   * In the n8n editor: **Workflow → Import from File** and select a `.json` from `workflows/`.
   * Or **Import from Clipboard** and paste the JSON.

4. **Create credentials** used by the workflow:

   * Example: *HTTP Basic Auth (AbuseIPDB)*, *OAuth2 (Jira)*, *Header Auth (VirusTotal)*, *Webhook URLs* for Slack/Discord.

5. **Run** the workflow:

   * Click **Execute** in the editor to test with sample inputs.
   * For production, set **Triggers** (Cron/Webhook/Schedule) and **Activate** the workflow.

---

## 🔐 Environment variables & credentials

The JSON files reference variables like:

* `{{ $env.VT_API_KEY }}`
* `{{ $env.ABUSEIPDB_API_KEY }}`
* `{{ $env.SHODAN_API_KEY }}`
* `{{ $env.SLACK_WEBHOOK_URL }}`
* `{{ $env.DISCORD_WEBHOOK_URL }}`

Add these to your n8n environment (Docker `--env-file`, Host env, or n8n Cloud secrets) and wire them in **Credentials**.

> Never commit real keys. Use `.env.example` as a template only.

---

## 🧩 Featured workflows

| Workflow                              | Category   | Input                       | Output                           |
| ------------------------------------- | ---------- | --------------------------- | -------------------------------- |
| `vt_hash_enrichment.json`             | Enrichment | File hash (MD5/SHA1/SHA256) | VT verdict, detections, links    |
| `abuseipdb_ip_reputation.json`        | Enrichment | IP address                  | Confidence score, report summary |
| `shodan_host_footprint.json`          | Enrichment | IP/Host                     | Open ports, banners, tags        |
| `slack_daily_threat_digest.json`      | Alerting   | IOC list / RSS / API        | Formatted Slack message          |
| `discord_alert_on_malicious_ioc.json` | Alerting   | Webhook event               | Discord embed with verdicts      |
| `thehive_auto_case_from_webhook.json` | Case Mgmt  | Webhook alert               | New case with observables        |
| `jira_incident_from_alert.json`       | Case Mgmt  | Alert payload               | Jira issue with fields & labels  |
| `json_normalize_ioc_list.json`        | Utilities  | Mixed IOC JSON              | Cleaned, typed IOC array         |
| `http_retry_backoff_subflow.json`     | Utilities  | Any HTTP call               | Resilient call with jitter       |

> Tip: Chain **Utilities** subflows inside your own projects to keep things DRY.

---

## 🧪 Testing

* Use the **“Execute Workflow”** button with the provided **sample payloads** (see `docs/usage-notes.md`).
* For webhooks, fire test events with:

  ```bash
  curl -X POST <your-n8n-webhook-url> \
    -H 'Content-Type: application/json' \
    -d '{"indicator":"8.8.8.8","type":"ip"}'
  ```

---

## 🛠️ Development notes

* Prefer **HTTP Request** + **Function** nodes for portability.
* Wrap vendor specifics behind **sub-workflows** when possible.
* Implement **rate-limit handling** (e.g., `Retry-After`) and **circuit breakers**.
* Use **Error Trigger** nodes for on-fail notifications.
* Keep **PII** out of logs; redact sensitive fields in messages.

---

## 🔒 Security & compliance

* Validate all incoming data (webhooks can be abused).
* Respect API **ToS** and **rate limits**.
* Store secrets in **n8n Credentials** or environment variables—**never in JSON**.
* Review workflows before enabling in production; add approvals for any “active response”.

---

## 🤝 Contributing

1. Fork the repo and create a feature branch.
2. Add your workflow JSON under the appropriate folder.
3. Include a short **doc block** at the top of the JSON (description, required creds, inputs/outputs).
4. Update this README’s **Featured workflows** table if applicable.
5. Submit a PR with screenshots or a short demo GIF if possible.

---

## 🧰 Roadmap

* More MISP & TheHive integrations (observables, tasks sync)
* Sigma/Sysmon parsing flows
* Enrichment multiplexers with **Queue**/RabbitMQ
* Tines/XSOAR migration helpers

---

## ❓ FAQ

* **Which n8n version is required?**
  Target: v1.40+ (older may work, but not tested).

* **Do these workflows send data to third parties?**
  Only when you configure nodes that call external APIs. Review each JSON before use.

* **How do I swap APIs (e.g., from VirusTotal to Hybrid-Analysis)?**
  Replace the HTTP nodes and keep the downstream mapping. Use the utilities subflows for common formatting.

---

## 📝 License

MIT © You and Contributors. See [LICENSE](./LICENSE) for details.

---

## 📣 Disclaimer

These workflows are provided **as-is**, without warranty. Use responsibly and only against systems you own or are authorized to test.
