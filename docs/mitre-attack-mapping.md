# MITRE ATT&CK Mapping and Detection Opportunities

## Overview

This document maps security monitoring capabilities in the SOC Home Lab to relevant MITRE ATT&CK techniques.

The mapping identifies techniques that can be investigated using the Windows Security logs and Sysmon telemetry collected by Splunk.

**Important:** These are detection opportunities and telemetry mappings. They do not mean that an actual attack using these techniques has been performed.

## Detection Mapping

| MITRE ATT&CK Technique | Technique ID | Relevant Telemetry | Investigation Goal | Status |
|---|---|---|---|---|
| Brute Force | T1110 | Windows Event ID 4625 | Investigate repeated failed authentication attempts | Search written |
| Valid Accounts | T1078 | Windows Event ID 4624 | Review successful logons and account activity | Search written |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 | Sysmon Event ID 1 | Investigate suspicious command-shell execution | Telemetry collected; dedicated search not yet written |
| System Services: Service Execution | T1569.002 | Windows System Event ID 7045, Sysmon Event ID 1 | Investigate suspicious service installation and execution | Planned |

## Splunk Investigation Searches

### 1. Failed Authentication Activity

Relevant technique: Brute Force (T1110)

```spl
index=main EventCode=4625
| stats count by host Account_Name Source_Network_Address
| sort - count
```

Investigation:
- Identify accounts with repeated failures.
- Review source addresses.
- Examine the time period and surrounding events.
- Determine whether the activity is legitimate or suspicious.

A failed login alone does not establish a brute-force attack.

### 2. Successful Authentication Activity

Relevant technique: Valid Accounts (T1078)

```spl
index=main EventCode=4624
| stats count by host Account_Name Logon_Type
| sort - count
```

Investigation:
- Identify accounts logging into endpoints.
- Review logon types.
- Look for unusual account or authentication patterns.

A successful login alone does not establish malicious use of a valid account.

### 3. Process Creation Activity

Relevant telemetry: Sysmon Event ID 1

```spl
index=main EventCode=1
| stats count by host Image ParentImage
| sort - count
```

Investigation:
- Identify executed processes.
- Examine parent-child process relationships.
- Look for unusual process execution patterns.

Process creation is useful for investigating multiple ATT&CK techniques, but Event ID 1 alone does not prove a specific technique occurred.

## Current Lab Coverage

- Windows authentication monitoring: Implemented and validated.
- Sysmon process creation monitoring: Implemented and validated.
- Centralized log collection: Implemented and validated.
- ATT&CK detection mapping: Initial mapping documented.
- Adversary simulation: Planned.
- Advanced behavioral detection: Planned.

## Future Improvements

- Perform controlled Atomic Red Team simulations to validate each mapping.
- Develop detection rules for suspicious PowerShell activity.
- Investigate service creation and persistence.
- Evaluate process injection detection (T1055), which requires Sysmon Event IDs 8 and 10 in the Sysmon configuration.
- Update the Status column as each technique is validated.
- Document false positives and investigation findings.

## Reference

MITRE ATT&CK:
https://attack.mitre.org/
