# MITRE ATT&CK Mapping and Detection Opportunities

## Overview

This document maps security monitoring capabilities in the SOC Home Lab to relevant MITRE ATT&CK techniques.

The mapping identifies techniques that can be investigated using the Windows Security logs and Sysmon telemetry collected by Splunk.

**Important:** Unless a row's status says "Validated with simulation," these are detection opportunities and telemetry mappings. They do not mean that an actual attack using these techniques has been performed.

## Detection Mapping

| MITRE ATT&CK Technique | Technique ID | Relevant Telemetry | Investigation Goal | Status |
|---|---|---|---|---|
| Brute Force | T1110 | Windows Event ID 4625 | Investigate repeated failed authentication attempts | Search written; scheduled alert configured and tested (DC01 - Repeated Failed Login Attempts) |
| Valid Accounts | T1078 | Windows Event ID 4624 | Review successful logons and account activity | Search written |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 | Sysmon Event ID 1 | Investigate suspicious command-shell execution | Validated with simulation ([report](atomic-red-team-test-01.md)) |
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

A scheduled alert, "DC01 - Repeated Failed Login Attempts," flags 2 or more failed logins within 5 minutes. The threshold is set low for lab testing and would be higher in a production environment. Machine accounts (names ending in `$`) generate background failures and can inflate counts.

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
index=main source="*Sysmon*" EventCode=1
| stats count by host Image ParentImage
| sort - count
```

Investigation:
- Identify executed processes.
- Examine parent-child process relationships.
- Look for unusual process execution patterns.

The Sysmon source filter is needed because Windows System logs also use Event ID 1 for an unrelated event. Process creation is useful for investigating multiple ATT&CK techniques, but Event ID 1 alone does not prove a specific technique occurred.

### 4. Command Shell Execution (validated)

Relevant technique: Windows Command Shell (T1059.003)

```spl
index=main source="*Sysmon*" EventCode=1 Image="*cmd.exe" ParentImage="*powershell.exe" CommandLine="*/c*" (CommandLine="*.bat*" OR CommandLine="*.cmd*")
| table _time host User CommandLine ParentImage ParentCommandLine
| sort - _time
```

Investigation:
- Review which account launched the shell and from which parent process.
- Check the script's location and contents.
- Compare against known administrative or scheduled activity.

Validated against Atomic Red Team test T1059.003 #1. See the [full report](atomic-red-team-test-01.md).

## Current Lab Coverage

- Windows authentication monitoring: Implemented and validated.
- Sysmon process creation monitoring: Implemented and validated on DC01 and CLIENT01.
- Centralized log collection: Implemented and validated.
- ATT&CK detection mapping: Initial mapping documented.
- Adversary simulation: One controlled test completed (T1059.003).
- Scheduled alerting: One alert configured and tested (failed logins).
- Advanced behavioral detection: Planned.

## Future Improvements

- Perform controlled Atomic Red Team simulations for the remaining techniques.
- Test PowerShell activity (T1059.001) and develop detection rules for it.
- Create a scheduled alert for the validated command-shell detection.
- Investigate service creation and persistence.
- Evaluate process injection detection (T1055), which requires Sysmon Event IDs 8 and 10 in the Sysmon configuration.
- Document false positives and investigation findings.

## Reference

MITRE ATT&CK:
https://attack.mitre.org/
