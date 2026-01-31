<img width="1536" height="1024" alt="ChatGPT Image Jan 31, 2026, 03_26_30 PM" src="https://github.com/user-attachments/assets/f5869430-03ca-4b65-b527-0025245a8529" />

#  Vulnerability Management Scan Agent 2.0

## Overview

This project is a **Vulnerability Management Orchestration Tool (Prototype)** designed to demonstrate how real-world security automation can be safely wrapped in an analyst-friendly control layer.

It transforms a traditionally **CLI-driven vulnerability scanning workflow** into a **guided, auditable desktop experience**, while preserving the underlying automation logic used in production environments.

The application wraps an existing Python backend (Tenable.io scan execution and Jira ticketing automation) inside a **PySide6 (Qt) desktop GUI**, allowing security analysts to configure, authorize, and monitor scans **without interacting directly with scripts or command-line arguments**.

While this project is a prototype built for demonstrative and portfolio purposes, it is intentionally designed around **real Tenable and Jira APIs, real data flows, and realistic security workflows**—not mocked examples or simulated logic.

The core design principle is **Human-in-the-Loop automation**:  
automation accelerates security operations, but humans remain explicitly responsible for authorizing risk-impacting actions.

---

## Prototype Scope & Intent

This project is **not a commercial SaaS product** or production deployment.

It is a **working prototype** created to demonstrate:

- How vulnerability scanning automation is structured in real environments
- How analysts safely interact with powerful backend tooling
- How guardrails, approvals, and visibility are enforced in security workflows
- How SOC, Vulnerability Management, and GRC concerns intersect in tooling design

All interactions follow **real execution paths** (Tenable scans, CSV exports, Jira ticket creation logic).  
No fake data, mocked APIs, or placeholder logic are used to represent system behavior.

---
## 🎥 Click to Watch the Demo Video ⬇️

<a href="https://youtu.be/C7uUVBkl3-s" target="_blank">
  <img width="1427" height="837" alt="Tenable Scan Agent Demo" src="https://github.com/user-attachments/assets/b9bfee95-2334-4be1-9959-589feeaec354">
</a>



## Who This Is For

This project is designed to reflect real-world security operations roles:

- **SOC Analysts (Tier 2)**  
  Initiate or support scoped vulnerability scans during investigations to validate exposure, without requiring Python or Tenable API expertise.

- **Vulnerability Management Engineers**  
  Verify remediation, re-scan assets, and automatically generate Jira tickets in a consistent, repeatable way.

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

This project addresses those gaps by **wrapping real automation in an analyst-first control layer**, without altering or oversimplifying the underlying logic.

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
- Logs are translated into a **human-readable mission timeline**  
  (e.g., *Scan Created → Scanning → Exporting → Jira Posting*).
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

