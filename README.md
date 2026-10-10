# AI Data Analyst Agent & Automated Data-Quality Framework

Two connected projects from my role as a Data Analyst (Automation & AI): an **automated data-quality framework** that reveals the *true* success rate of a mobile app's telemetry, and an **AI data analyst agent** that runs that analysis on its own and answers free-form business questions in plain English.

> **All figures, reports and demos in this repo use synthetic / illustrative data.** No production data, credentials or source code from the company are included. The agent accesses data strictly **read-only**.

![AI data analyst agent architecture](<02-ai-data-analyst-agent/ai-agent-architecture.png>)

---

## 1. Automated Data-Quality Framework

A suite of SQL checks that catches "fake" successes in event telemetry (implausible durations, timestamp errors, collection gaps, app defects) and **reclassifies** them, so the headline success rate reflects reality. Events are never deleted, only reclassified, and the suite validates its own numbers (PASS/FAIL self-checks).

**Skills:** advanced SQL (CTEs, window functions, FILTER aggregates), data-quality engineering, metric design, Python / pandas reporting.

| What | File |
|---|---|
| Case study | [Case study (PDF)](<01-data-quality-framework/Case Study — Automated Data-Quality Framework (HEXA VPN).pdf>) · [Markdown](<01-data-quality-framework/Case Study — Automated Data-Quality Framework (HEXA VPN).md>) |
| SQL highlights | [SQL file](<01-data-quality-framework/SQL Highlights/SQL Highlights — DQ Framework (HEXA VPN).sql>) · [PDF](<01-data-quality-framework/SQL Highlights/SQL Highlights — DQ Framework (HEXA VPN).pdf>) |
| Reports (synthetic) | [DQ Checks Explained](<01-data-quality-framework/Sanitized Report/DQ Checks Explained.pdf>) · [DQ Impact Report](<01-data-quality-framework/Sanitized Report/DQ Impact Report.pdf>) |
| Architecture | [Diagram](<01-data-quality-framework/dq-framework-architecture.png>) |
| Demo | [Preview (GIF)](<01-data-quality-framework/demo-dq-checks.gif>) · [Full recording (MP4, download)](<01-data-quality-framework/demo-dq-checks.mp4>) · [Interactive demo (download & open in a browser)](<01-data-quality-framework/DQ Impact — Interactive Demo (app_version 1.13.5, synthetic).html>) |

![Data-quality framework architecture](<01-data-quality-framework/dq-framework-architecture.png>)

**Demo (2× speed preview — [download the full MP4](<01-data-quality-framework/demo-dq-checks.mp4>)):**

![Data-quality checks demo](<01-data-quality-framework/demo-dq-checks.gif>)

---

## 2. AI Data Analyst Agent

A **LangGraph** agent that reproduces the full data-quality workflow automatically and answers free-form questions about the same data:

`route → generate SQL → validate (guard + EXPLAIN) → execute (read-only) → verify → explain`, with a self-repair loop that feeds database errors back to the model.

- **Generative text-to-SQL over a semantic layer** of documented metric definitions, so answers are business-correct, not just valid SQL
- **Guardrails:** SELECT-only SQL guard, EXPLAIN check before execution, injected LIMIT and timeout, SELECT-only database user
- **Privacy by design:** the cloud model sees the schema and question only; results are always explained by a local model; PII scanning on outbound text
- **Evaluation:** answers checked question by question against the database, plus a fixed regression set
- **Automation:** scheduled, colour-coded HTML reports (Comparison + Action Plan)

**Stack:** Python · LangGraph / LangChain · Groq (cloud, SQL generation) + local Ollama (explanations) · PostgreSQL (read-only) · ChromaDB · Presidio · Streamlit · Plotly · n8n · Docker

> **Model history:** the agent was originally built when Groq still served **Llama 3.3 70B**, with **Qwen 2.5** running locally via Ollama — so the case study, screenshots and demo show those models. Groq has since retired Llama 3.3 70B, so the cloud model was switched to **gpt-oss-120b**, and the local model was upgraded to **Qwen3 Coder 30B**. The architecture is model-agnostic: swapping models is a config change, re-checked against the same regression set.

| What | File |
|---|---|
| Case study | [Case study (PDF)](<02-ai-data-analyst-agent/Case Study — Automated Data Analyst AI Agent (HEXA VPN).pdf>) · [Markdown](<02-ai-data-analyst-agent/Case Study — Automated Data Analyst AI Agent (HEXA VPN).md>) |
| Architecture | [Diagram (PNG)](<02-ai-data-analyst-agent/ai-agent-architecture.png>) · [PDF](<02-ai-data-analyst-agent/ai-agent-architecture.pdf>) |
| App screenshots | [Chat with chart](<02-ai-data-analyst-agent/App Screenshots/App — Chat with chart (illustrative).png>) · [Full analysis running](<02-ai-data-analyst-agent/App Screenshots/App — Full Analysis running (illustrative).png>) |
| Generated reports (synthetic) | [Comparison Report](<02-ai-data-analyst-agent/Sanitized Report/Comparison Report.pdf>) · [Action Plan](<02-ai-data-analyst-agent/Sanitized Report/Action Plan.pdf>) |
| Demo | [Preview (GIF)](<02-ai-data-analyst-agent/demo-ai-agent.gif>) · [Full recording (MP4, download)](<02-ai-data-analyst-agent/demo-ai-agent.mp4>) · [Interactive run replay (download & open in a browser)](<02-ai-data-analyst-agent/agent-run-demo.html>) |

**Demo (2× speed preview — [download the full MP4](<02-ai-data-analyst-agent/demo-ai-agent.mp4>)):**

![AI data analyst agent demo](<02-ai-data-analyst-agent/demo-ai-agent.gif>)

---

*Portfolio repository — documentation, synthetic reports, diagrams and demos only.*
