# Tenable Scan Agent – Analyst Console (GUI-Orchestrated Vulnerability Management)

## Overview

This project is a **Vulnerability Management Orchestration Tool** that transforms a traditionally CLI-driven workflow into a **safe, guided, and auditable analyst experience**.

It wraps an existing Python backend (Tenable.io scanning + Jira ticketing automation) inside a **PySide6 (Qt) desktop application**, enabling security teams to run scans, monitor progress, and generate remediation tickets **without touching scripts or command-line arguments**.

The core design principle is **Human-in-the-Loop automation**: automation accelerates work, but humans remain explicitly in control of risk-impacting actions.

---

## Who This Is For

This project is designed to reflect real-world security operations roles:

- **SOC Analysts (Tier 1 / Tier 2)**  
  Run ad-hoc scans against suspicious endpoints without needing Python or Tenable API expertise.

- **Vulnerability Management Engineers**  
  Validate fixes, re-scan assets, and automatically generate Jira tickets in a consistent, repeatable way.

- **GRC / Compliance Teams**  
  Capture execution evidence (logs, artifacts, timestamps) to support audits and compliance requirements.

---

## The Problem This Solves

Traditional vulnerability automation often looks like this:

- Powerful scripts
- Minimal guardrails
- Little visibility once execution starts
- High risk of misconfiguration or accidental scans
- Manual handoffs between Tenable and Jira

This project addresses those gaps by **wrapping automation in an analyst-first control layer**.

---

## Key Capabilities (High Level)

### Guided, Safe Execution
- Analysts are guided step-by-step through configuration.
- Invalid inputs are blocked before execution.
- **Explicit authorization is required** before any scan is launched.

### Human-in-the-Loop Automation
- The system will not trigger Tenable scans silently.
- Analysts must actively confirm intent, reducing operational risk.

### Real-Time Visibility
- Raw backend logs are streamed live.
- Logs are translated into a **human-readable mission timeline** (e.g., *Scan Created → Scanning → Exporting → Jira Posting*).
- No “black box” execution.

### Secure Credential Handling
- API keys are stored in `.env` files and **masked in the UI**.
- Secrets are passed via environment variables, never command-line arguments.
- Keys are never printed to logs or UI output.

### Unified Workflow
- One interface to:
  - Configure scan targets
  - Launch Tenable scans
  - Monitor execution
  - Export findings
  - Create Jira tickets
- Eliminates manual CSV exports and copy-paste workflows.

---

## Architecture (At a Glance)

This project follows a **decoupled, production-safe architecture**.

### Frontend (Analyst Console)
- Built with **PySide6 (Qt for Python)**.
- Event-driven and non-blocking.
- Responsible for:
  - Input validation
  - Process orchestration
  - State visualization
  - Secure configuration management

### Backend (Unmodified Logic)
- Existing Python scripts responsible for:
  - Tenable.io API interaction
  - Scan lifecycle management
  - CSV export and normalization
  - Jira ticket creation
- Runs as an **isolated subprocess**, preserving CLI usability.

### Why Subprocess Isolation Matters
- Prevents UI freezes.
- Avoids threading hazards.
- Keeps backend reusable for automation pipelines or CI workflows.
- Mirrors how real SOC tooling wraps legacy automation safely.

---

## How the GUI Controls the Backend (Conceptual)

1. The GUI launches the backend using a subprocess.
2. Runtime settings (targets, flags) are injected via environment variables.
3. Backend stdout/stderr is streamed in real time.
4. The GUI programmatically satisfies CLI confirmation prompts **only after analyst approval**.
5. Logs are parsed to update visual execution state.

This approach keeps automation powerful **without removing human accountability**.

---

## Security & Compliance Design

- **Non-destructive `.env` updates**  
  Configuration changes preserve comments and structure for audit clarity.

- **Masked secrets**  
  Keys are never displayed in full, logged, or exposed via process arguments.

- **Execution artifacts**  
  Each run produces timestamped logs and cached metadata for traceability.

This mirrors how enterprise security tooling is evaluated in regulated environments.

---

## Repository Structure (Simplified)

```text
├── gui/                  # Analyst Console (PySide6)
│   ├── app.py            # Main GUI entry point
│   ├── process_runner.py # Subprocess + threading control
│   ├── timeline_parser.py# Log-to-state translation
│   ├── env_manager.py    # Secure .env handling
│   └── ui_styles.py      # Custom Qt styling
├── tenable_scan_agent/   # Backend automation (CLI)
│   ├── main.py
│   ├── post_scan_to_jira.py
│   └── ...
└── runs/                 # Execution artifacts (logs, caches)
