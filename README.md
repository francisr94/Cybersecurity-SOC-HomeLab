# Cybersecurity SOC Home Lab

## Project Overview

This project documents the design, deployment, and configuration of a virtualized Security Operations Center (SOC) home lab built using Oracle VirtualBox.

The goal is to develop practical cybersecurity skills in network security, Windows system administration, centralized log collection, security monitoring, detection engineering, and incident investigation.

**Project Outcome:** Windows Security and Sysmon logs from two domain-joined systems are collected in Splunk Enterprise. I use Search Processing Language (SPL) to investigate authentication activity and process creation, validate detection logic through controlled testing, and generate alerts for repeated failed login attempts.

## Key Findings

| Investigation | Telemetry | Documentation |
|---|---|---|
| Command shell execution (T1059.003), validated with Atomic Red Team | Sysmon Event ID 1 | [Atomic Red Team Test 01](docs/atomic-red-team-test-01.md) |
| Repeated failed-login alerting | Windows Event ID 4625 | [Failed Login Alert Test](docs/alert-failed-login-test.md) |
| Successful authentication | Windows Event ID 4624 | [Splunk Detection Queries](queries/splunk-detection-queries.md) |
| End-to-end security investigation | Windows Security and Sysmon logs | [SOC Incident Investigation Report](docs/incident-investigation-report.md) |

## Lab Architecture

The environment runs in Oracle VirtualBox using an isolated internal network named `CorpLAN`.

| System | Operating System | IP Address | Purpose |
|---|---|---|---|
| pfSense | pfSense CE | 192.168.10.1 | Firewall and network gateway |
| DC01 | Windows Server 2022 | 192.168.10.10 | Active Directory and DNS |
| CLIENT01 | Windows Client | 192.168.10.100 | Domain-joined endpoint |
| SIEM01 | Ubuntu Server | 192.168.10.20 | Splunk SIEM |

## Technologies Used

- Oracle VirtualBox
- pfSense CE
- Windows Server 2022
- Active Directory Domain Services (AD DS)
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

### 2. Active Directory and DNS

- Installed and configured Active Directory Domain Services.
- Created the `corp.local` domain.
- Configured DNS services.
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
- Configured Splunk Universal Forwarder to collect Windows Security and Sysmon events.
- Verified Sysmon process creation events (Event ID 1).

### 5. Security Event Investigation

- Generated and investigated failed authentication events (Event ID 4625).
- Reviewed successful authentication events (Event ID 4624).
- Investigated Sysmon process creation activity.
- Developed SPL searches to examine and summarize security events.

### 6. Detection Validation and Alerting

- Executed a controlled Atomic Red Team test for Windows Command Shell (T1059.003) on CLIENT01.
- Confirmed the resulting Sysmon telemetry in Splunk.
- Configured a scheduled Splunk alert for repeated failed logins on DC01.
- Verified that the scheduled alert triggered during testing.
- Identified and corrected a clock offset on DC01 that affected alert detection timing.

## Detection Engineering

Documented detection searches and monitoring activities include:

- Failed login monitoring (Event ID 4625).
- Successful authentication activity (Event ID 4624).
- Sysmon process creation monitoring (Event ID 1).
- Windows security event investigation.
- Threshold-based failed login detection and scheduled alerting.
- Command shell execution detection validated with Atomic Red Team.

See [Splunk Detection Queries](queries/splunk-detection-queries.md).

## Project Documentation

| Resource | Description |
|---|---|
| [SIEM Implementation](docs/siem-implementation.md) | SIEM installation and configuration |
| [SOC Lab Network Topology](diagrams/soc-lab-network.md) | Network architecture |
| [Splunk Detection Queries](queries/splunk-detection-queries.md) | SPL searches and detection logic |
| [MITRE ATT&CK Mapping](docs/mitre-attack-mapping.md) | Mapping of detection activities to ATT&CK |
| [Atomic Red Team Test 01](docs/atomic-red-team-test-01.md) | Command shell detection validation |
| [Failed Login Alert Test](docs/alert-failed-login-test.md) | Scheduled alert testing |
| [SOC Analyst Technical Skills](docs/soc-analyst-skills.md) | Technical skills demonstrated |
| [SOC Incident Investigation Report](docs/incident-investigation-report.md) | Documented security investigation |
| [Screenshots and Lab Evidence](screenshots/) | Visual project evidence |

## Skills Demonstrated

- Network configuration and troubleshooting
- Firewall administration
- Windows Server administration
- Active Directory and DNS management
- Linux server administration
- SIEM deployment and configuration
- Endpoint telemetry collection
- Windows Security and Sysmon event analysis
- SPL searches and detection development
- Detection validation with Atomic Red Team
- Scheduled security alerting
- Security monitoring and technical documentation

## Planned Enhancements

The following activities are future development goals and have not yet been completed:

- Additional Atomic Red Team simulations, including PowerShell execution (T1059.001).
- Failed-login alerting for CLIENT01 and alert tuning.
- Suricata network intrusion detection.
- Advanced threat hunting exercises.
- Additional Kali Linux security testing.
- Expanded incident response exercises.

## Related Portfolio Projects

### 1. Python Network Traffic Monitoring and Alerting

[View Network Traffic Monitoring on GitHub](https://github.com/francisr94/Network-Traffic-Monitoring)

A Python-based network monitoring prototype developed in Kali Linux using TShark.

- Captures and analyzes ICMP traffic.
- Groups packet activity by source IP address.
- Generates structured JSON security alerts.
- Demonstrates threshold-based network monitoring.

### 2. Phishing Email Analysis

[View Phishing Email Analysis on GitHub](https://github.com/francisr94/Phishing-Email-Analysis)

A hands-on email security investigation project.

- Analyzes suspicious email headers.
- Investigates potential phishing indicators.
- Uses security research tools to evaluate suspicious activity.
- Documents investigation findings.

### 3. Failed Login Analyzer

[View Failed Login Analyzer on GitHub](https://github.com/francisr94/Failed-Login-Analyzer)

A Python-based tool for analyzing repeated failed authentication attempts.

- Analyzes Windows Event ID 4625 records.
- Applies configurable detection thresholds.
- Supports authentication log investigation.

## Project Status

**Current Stage:** Operational SOC monitoring lab with centralized Windows event collection, validated command-shell detection, and tested scheduled failed-login alerting.

This is an educational cybersecurity home lab developed to strengthen practical security operations, monitoring, detection engineering, and incident investigation skills.
