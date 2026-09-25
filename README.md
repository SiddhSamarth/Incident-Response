# Enterprise Incident Response Framework & Phishing Handling Playbook

A procedural incident response framework and operational playbook modeled after NIST SP 800-61r2 and SANS PICERL for detecting, analyzing, containing, and eradicating enterprise credential-harvesting phishing campaigns.

---

## Overview

Phishing remains the primary initial access vector (MITRE ATT&CK `T1566`) across enterprise environments. A successful response depends not only on automated email gateway controls, but on disciplined, repeatable procedural playbooks that guide SOC analysts, incident responders, and system administrators through rapid triage, containment, and recovery.

This repository documents an **end-to-end incident response framework** developed for a simulated enterprise-wide credential-harvesting attack. It outlines the end-to-end operational lifecycle from initial telemetry detection to user account isolation, firewall blocklisting, enterprise-wide inbox sweeping, and post-incident root-cause analysis.

---

## Scenario Architecture

* **Threat Vector:** Spear-phishing email masquerading as an urgent mandatory security notification from corporate IT.
* **Malicious Objective:** Credential harvesting via an external lookalike authentication portal.
* **Scope:** Multi-department cross-section to evaluate security awareness, reporting latency, and technical triage efficiency.

---

## The 6-Phase Incident Handling Lifecycle

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────────┐
│ 1. PREPARATION   │ ──> │ 2. IDENTIFICATION│ ──> │ 3. CONTAINMENT   │
│  - Gateway rules │     │  - SIEM alerts   │     │  - Mail quarantine
│  - User training │     │  - Header triage │     │  - Host isolation│
└──────────────────┘     └──────────────────┘     └─────────┬────────┘
                                                            │
┌──────────────────┐     ┌──────────────────┐               │
│6. LESSONS LEARNED│ <── │  5. RECOVERY     │ <── ┌─────────▼────────┐
│  - Root cause rep│     │  - Cred resets   │     │ 4. ERADICATION   │
│  - Policy tuning │     │  - Host unfreeze │     │  - Mailbox sweep │
└──────────────────┘     └──────────────────┘     │  - C2 blocklist  │
                                                  └──────────────────┘
```

### Phase 1: Preparation
* Implementation of SPF, DKIM, and DMARC enforcement records on mail transfer agents (MTAs).
* Pre-configured Mail Gateway quarantine rules and SIEM detection correlation queries.
* Pre-authorized emergency containment workflows and incident communication matrices.

### Phase 2: Identification & Triage
* **Automated Telemetry:** SIEM correlation alert triggering on multiple outbound connections to an unclassified domain within 60 seconds of inbound mail delivery.
* **Header Forensics:** Triage of RFC 822 email headers (`Return-Path`, `Authentication-Results`, `X-Originating-IP`) to identify sender infrastructure and spoofing indicators.
* **Landing Page Inspection:** Safe headless analysis of the fake login portal to extract C2 endpoints and credential-harvesting scripts.

### Phase 3: Containment
* **Immediate Email Quarantine:** Global search and quarantine across Microsoft 365 / Google Workspace tenant to purge identical subject lines and message hashes.
* **Network & Perimeter Isolation:** Egress firewall rule deployment blocking all outbound traffic to the attacker IP infrastructure and C2 domain.
* **Host Isolation:** Endpoint Detection & Response (EDR) network containment of workstations belonging to users who interacted with the malicious link.
* **Emergency Account Lockout:** Revocation of active refresh tokens and temporary lockout of credentials entered into the fake portal.

### Phase 4: Eradication
* **Organization-Wide Mailbox Sweep:** Automated script verification ensuring zero instances of the phishing payload persist in junk, trash, or forwarded folders.
* **Cache & Credential Flushing:** Local browser cache purge, active session invalidation, and removal of any dropped artifacts.

### Phase 5: Recovery
* **Secure Credential Reset:** Multi-factor authentication (MFA) re-registration and password rotation via out-of-band verification.
* **Controlled Endpoint Reconnection:** Phased release of isolated endpoints from EDR containment following clean malware/EDR scans.
* **Continuous Monitoring:** Heightened logging and threshold alerts on affected user accounts for 14 days.

### Phase 6: Post-Incident Activity & Lessons Learned
* **Root-Cause Analysis (RCA):** Compilation of formal incident timeline, Mean Time to Detect (MTTD), and Mean Time to Remediate (MTTR).
* **Defensive Tuning:** Updating mail filter regex rules, adding extracted IOCs to threat intelligence feeds, and refining employee training modules.

---

## Incident Response Checklist for SOC Analysts

| Stage | Action Item | Verification Mechanism |
| :--- | :--- | :--- |
| **Triage** | Confirm phishing payload and extract sender domain/IP | Header analysis & sandbox inspection |
| **Scope** | Identify all internal recipients who received or opened email | Mail trace & gateway transaction logs |
| **Contain** | Revoke active sessions for users who submitted credentials | Identity Provider (Azure AD / Okta) audit logs |
| **Block** | Inject C2 IPs and domain into perimeter firewall & proxy blocklists | DNS sinkhole & proxy rule logs |
| **Sweep** | Hard-delete all copies of the email from all tenant inboxes | Tenant PowerShell compliance search |
| **Report** | Generate incident report with MITRE ATT&CK technique mapping | SOC ticketing system / Wiki |

---

## Project Status & Methodology

* **Project Type:** Procedural Security Framework & Operational Playbook.
* **Implementation Note:** This repository serves as a structured operational guide and procedural blueprint for enterprise SOC teams. It does not contain automated executable scripts or production SIEM API connectors.

---

## Author & Links

* **Author:** Siddh Samarth
* **GitHub:** [@SiddhSamarth](https://github.com/SiddhSamarth)
* **Portfolio:** [siddhsamarth.in](https://siddhsamarth.in)
* **LinkedIn:** [samarthsiddh](https://www.linkedin.com/in/siddhsamarth/)
