# Cybersecurity SOC Home Lab

![Network Diagram](diagrams/network-topology.png)

A self-contained, segmented enterprise environment built in VirtualBox to simulate the core
functions of a Security Operations Center: perimeter defense, Active Directory identity
management, centralized log collection, and end-to-end attack detection and remediation.

## Overview

This lab was built to practice the full SOC analyst workflow — not just standing up tools,
but using them the way a real security team would: generate an event, find it in the SIEM,
and close the gap with a concrete defensive control.

**Stack:** pfSense · Windows Server 2022 (Active Directory) · Windows 10/11 · Splunk Enterprise ·
Sysmon · Kali Linux · Atomic Red Team

## Lab Architecture

| Component | Role | Network Segment |
|---|---|---|
| pfSense | Perimeter firewall / router | WAN ↔ CorpLAN |
| Windows Server 2022 | Active Directory Domain Controller | CorpLAN |
| Windows 10/11 | Domain-joined client endpoint | CorpLAN |
| Ubuntu + Splunk Enterprise | Centralized SIEM / log collection | CorpLAN (management) |
| Kali Linux | Adversary simulation | External/attacker segment |

See [`diagrams/network-topology.png`](diagrams/network-topology.png) for the full topology.

## What's in this repo

- **`diagrams/`** — network topology diagram (draw.io source + exported PNG)
- **`screenshots/`** — build evidence for each phase of the lab
  - `01-pfsense/` — firewall and network segmentation setup
  - `02-active-directory/` — domain controller, GPOs, domain-joined client
  - `03-splunk-setup/` — SIEM install, forwarders, Sysmon ingestion
  - `04-attack-detection/` — the case study below, step by step
- **`queries/`** — saved Splunk/SPL search queries used in the case study
- **`docs/`** — detailed write-up of the attack & detection case study

## Attack & Detection Case Study

**Summary:** [one sentence describing the attack you ran, e.g. "Simulated a brute-force
authentication attack from an external attacker segment against a domain-joined Windows
client, detected via Splunk, and remediated with a pfSense firewall rule."]

Full write-up: [`docs/attack-case-study.md`](docs/attack-case-study.md)

| Step | Description |
|---|---|
| 1. Adversarial Vector | Attack executed from Kali Linux against the CorpLAN environment |
| 2. SIEM Ingestion & Query | Event isolated in Splunk using Sysmon/Windows Event Log telemetry |
| 3. Remediation | Defensive control implemented (GPO / pfSense rule) and verified |

## Key Skills Demonstrated

- Network segmentation and perimeter firewall configuration
- Active Directory deployment and Group Policy hardening
- SIEM pipeline design: log forwarding, parsing, and search
- Threat detection mapped to [MITRE ATT&CK](https://attack.mitre.org/)
- Incident documentation and remediation verification

## Future Improvements

- [ ] Add a second detection scenario (e.g. lateral movement or persistence)
- [ ] Build Splunk dashboards for the mapped ATT&CK techniques
- [ ] Add Suricata/Zeek for network-based detection alongside host telemetry

---
**Author:** Francisco Reyes | [LinkedIn](https://www.linkedin.com/in/yourprofile) | MS Cybersecurity, Thomas Edison State University
