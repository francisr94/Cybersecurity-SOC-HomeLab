# SIEM Implementation and Validation

## 1. Environment

The SIEM environment consists of three primary systems:

| Host | IP Address | Function |
|---|---|---|
| DC01 | 192.168.10.10 | Windows Server 2022 / Active Directory |
| CLIENT01 | 192.168.10.100 | Domain-joined Windows endpoint |
| SIEM01 | 192.168.10.20 | Ubuntu Server / Splunk Enterprise |

Network: CorpLAN (192.168.10.0/24)

## 2. Splunk Deployment

Splunk Enterprise was installed on SIEM01.

Services:
- Splunk Web: TCP 8000
- Splunk receiving: TCP 9997

## 3. Endpoint Configuration

Microsoft Sysmon was installed on DC01 and CLIENT01.

The SwiftOnSecurity Sysmon configuration was applied to both endpoints.

Splunk Universal Forwarder was installed on both Windows systems.

Configured event collection:
- Security
- System
- Application
- Microsoft-Windows-Sysmon/Operational

## 4. Detection Validation

### Failed Authentication

Generated an intentional failed login on CLIENT01.

Windows Security Event ID: 4625

The event was successfully located in Splunk.

### Process Creation

Executed Notepad on CLIENT01 and DC01.

Sysmon Event ID: 1

Process creation events were successfully located in Splunk.

## 5. Outcome

Validated centralized log collection and endpoint telemetry from both Windows systems.

Demonstrated basic security event investigation using Splunk Search Processing Language (SPL).

## 6. Next Steps

- Advanced correlation searches
- Network intrusion detection with Suricata
- Threat hunting
- MITRE ATT&CK mapping
- Automated incident response
