# Cybersecurity SOC Home Lab

## Project Overview

This project documents the design, deployment, and configuration of a virtualized Security Operations Center (SOC) home lab.

The purpose of this lab is to develop practical cybersecurity skills in network security, Windows system administration, centralized log collection, security monitoring, and threat detection.

**Outcome:** Windows Security and Sysmon logs from two domain-joined systems are collected in Splunk, where I investigate authentication activity and process creation using SPL, validate a detection with a simulated attack, and alert on repeated failed logins.

## Key Findings

| Investigation | Telemetry | Documented in |
|---|---|---|
| Command shell execution (T1059.003), validated with Atomic Red Team | Sysmon Event ID 1 | [Atomic Red Team Test 01](docs/atomic-red-team-test-01.md) |
| Failed-login alerting (T1110) | Windows Event ID 4625 | [Failed Login Alert Test](docs/alert-failed-login-test.md) |
| Successful authentication | Windows Event ID 4624 | [Splunk Detection Queries](queries/splunk-detection-queries.md) |
| End-to-end investigation | Windows Security and Sysmon logs | [SOC Incident Investigation Report](docs/incident-investigation-report.md) |

## Lab Architecture

The environment runs in Oracle VirtualBox using an isolated internal network called `CorpLAN`.

| System | Operating System | IP Address | Purpose |
|---|---|---|---|
| pfSense | pfSense CE | 192.168.10.1 | Firewall and gateway |
| DC01 | Windows Server 2022 | 192.168.10.10 | Active Directory and DNS |
| CLIENT01 | Windows Client | 192.168.10.100 | Domain-joined endpoint |
| SIEM01 | Ubuntu Server | 192.168.10.20 | Splunk SIEM |

## Technologies Used

- Oracle VirtualBox
- pfSense CE
- Windows Server 2022
- Active Directory Domain Services
- Windows DNS
- Ubuntu Server
- Splunk Enterprise
- Splunk Universal Forwarder
- Microsoft Sysmon
- Atomic Red Team
- Splunk Search Processing Language (SPL)

## Implemented Components

### 1. Network Security
- Configured pfSense as the lab network gateway.
- Established an isolated VirtualBox internal network.
- Verified network connectivity between lab systems.

### 2. Active Directory
- Installed and configured Active Directory Domain Services.
- Created the `corp.local` domain.
- Configured DNS.
- Joined CLIENT01 to the domain.
- Verified domain authentication.

### 3. SIEM Deployment
- Installed Splunk Enterprise on Ubuntu Server.
- Configured Splunk Web on TCP port 8000.
- Enabled receiving on TCP port 9997.
- Installed Splunk Universal Forwarder on both Windows endpoints.

### 4. Endpoint Monitoring
- Installed Microsoft Sysmon on DC01 and CLIENT01.
- Applied a Sysmon configuration.
- Configured the Splunk Universal Forwarder to send Windows Security and Sysmon logs to Splunk.
- Verified Sysmon process creation events (Event ID 1).

### 5. Security Event Investigation
- Generated and investigated failed authentication events (Event ID 4625).
- Reviewed successful authentication events (Event ID 4624).
- Investigated Sysmon process creation activity.
- Used SPL searches to summarize security events.

### 6. Detection Validation and Alerting
- Ran a controlled Atomic Red Team test (T1059.003) on CLIENT01 and confirmed the resulting Sysmon events in Splunk.
- Configured a scheduled Splunk alert for repeated failed logins on DC01 and confirmed that it fires.
- Found and corrected a clock offset on DC01 that was delaying alert detection.

## Detection Engineering

Documented SPL searches and alerts include:

- Failed login monitoring.
- Successful login activity.
- Sysmon process creation.
- Windows security event overview.
- Threshold-based failed login detection, including a scheduled alert.
- Command shell execution (validated with Atomic Red Team).

See [Splunk Detection Queries](queries/splunk-detection-queries.md).

## Project Documentation

- [SIEM Implementation](docs/siem-implementation.md)
- [SOC Lab Network Topology](diagrams/soc-lab-network.md)
- [Splunk Detection Queries](queries/splunk-detection-queries.md)
- [MITRE ATT&CK Mapping](docs/mitre-attack-mapping.md)
- [Atomic Red Team Test 01](docs/atomic-red-team-test-01.md)
- [Failed Login Alert Test](docs/alert-failed-login-test.md)
- [SOC Analyst Technical Skills](docs/soc-analyst-skills.md)
- [SOC Incident Investigation Report](docs/incident-investigation-report.md)
- [Screenshots and Lab Evidence](screenshots/)

## Skills Demonstrated

- Network configuration and troubleshooting
- Firewall administration
- Windows Server administration
- Active Directory and DNS management
- Linux server administration
- SIEM deployment and configuration
- Endpoint telemetry collection
- Log analysis and investigation
- SPL detection searches
- Detection validation with Atomic Red Team
- Scheduled alerting
- Security monitoring and documentation

## Planned Enhancements

The following activities are future development goals, not yet completed:

- Additional Atomic Red Team simulations (for example, PowerShell, T1059.001)
- Failed-login alert for CLIENT01 and alert tuning
- Suricata network intrusion detection
- Advanced threat hunting
- Kali Linux security testing
- Incident response exercises

## Project Status

**Current stage:** SOC monitoring with centralized Windows event collection, one validated detection, and one tested alert.

This is an educational home lab built for hands-on cybersecurity training and professional portfolio development.
