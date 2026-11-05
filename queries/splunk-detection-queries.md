# Splunk Detection Queries

These SPL searches were developed and tested in the Enterprise SOC Monitoring Home Lab.

## 1. Failed Login Attempts

**Windows Event ID:** 4625

```spl
index=main EventCode=4625
| stats count by host Account_Name Source_Network_Address
| sort - count
```

Purpose: Identify failed authentication attempts.

## 2. Successful Login Activity

**Windows Event ID:** 4624

```spl
index=main EventCode=4624
| stats count by host Account_Name Logon_Type
| sort - count
```

Purpose: Review successful authentication activity.

## 3. Sysmon Process Creation

**Sysmon Event ID:** 1

```spl
index=main EventCode=1
| stats count by host Image ParentImage
| sort - count
```

Purpose: Investigate process execution and parent-child relationships.

## 4. Windows Security Event Overview

```spl
index=main EventCode IN (4624,4625,4672,4688)
| stats count by host EventCode
| sort - count
```

Purpose: Summarize selected Windows security events.

## 5. Failed Login Detection Threshold

```spl
index=main EventCode=4625
| stats count by host Account_Name Source_Network_Address
| where count >= 5
```

Purpose: Identify accounts and sources with five or more failed login events within the selected search window.

**Note:** This is a basic threshold-based detection, not proof of a brute-force attack. Validate the source, account, time window, and surrounding activity before drawing conclusions.
