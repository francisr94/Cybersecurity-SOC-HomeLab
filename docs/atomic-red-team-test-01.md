# Atomic Red Team Test 01: Windows Command Shell (T1059.003)

## Objective

Validate that the SOC Home Lab can detect command-shell execution using Sysmon telemetry forwarded to Splunk.

## Technique

| Field | Value |
|---|---|
| MITRE ATT&CK Technique | Command and Scripting Interpreter: Windows Command Shell |
| Technique ID | T1059.003 |
| Atomic Test | T1059.003, Test #1 |
| Tactic | Execution |

## Environment

- Target system: [CLIENT01, Windows version]
- Telemetry: Sysmon [version], forwarded to Splunk
- Splunk index: main
- Test date and time: [date, time, time zone]
- Isolation: [e.g., isolated VM network behind pfSense, snapshot taken before the test]

## What I Ran

```powershell
Invoke-AtomicTest T1059.003 -TestNumbers 1
Invoke-AtomicTest T1059.003 -TestNumbers 1 -Cleanup
```

[One or two sentences describing what the test does, based on the -ShowDetails output.]

![Atomic test running on CLIENT01](../screenshots/atomic-t1059-003-run.png)

## What the Logs Showed

| Field | Value |
|---|---|
| Event ID | [Sysmon Event ID 1] |
| Image | [path of the executed process] |
| CommandLine | [command line observed] |
| ParentImage | [parent process] |
| User | [account that ran it] |
| Timestamp | [time] |

[Two or three sentences on what you saw and why it is consistent with the technique.]

![Splunk results for the test](../screenshots/atomic-t1059-003-splunk.png)

## Detection Query

```spl
index=main EventCode=1 host=CLIENT01
| search Image="*cmd.exe" OR Image="*powershell.exe"
| table _time host User Image CommandLine ParentImage ParentCommandLine
| sort _time
```

[Note any changes you made to the query and why, such as field names that differed in your environment.]

## Alert Logic

[Describe how this could become an alert. For example: trigger when cmd.exe is launched by an unexpected parent process such as winword.exe or a script host, or when the command line contains specific suspicious patterns. State the thresholds, the time window, and the severity you would assign.]

## False Positives

[List legitimate activity that could trigger this detection, such as administrators using cmd.exe, software installers, login scripts, or scheduled tasks. Explain how you would tell them apart from malicious use, for example by parent process, user, or time of day.]

## Limitations

- This was a single controlled test in an isolated lab.
- [Anything the detection missed or could not distinguish.]
- Event ID 1 alone does not prove malicious activity. Context is required.

## Result

| Item | Outcome |
|---|---|
| Telemetry generated | [Yes/No] |
| Visible in Splunk | [Yes/No] |
| Detection query worked | [Yes/No/Needed tuning] |
| ATT&CK status | [Validated with simulation / Partially validated] |

## Next Steps

- [e.g., Test T1059.001 (PowerShell)]
- [e.g., Create a Splunk alert from the detection query]
- [e.g., Tune the query to reduce false positives]
