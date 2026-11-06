
# SOC Incident Investigation Report

## Case Information

| Field | Details |
|---|---|
| Case ID | SOC-001 |
| Incident Type | Repeated Failed Authentication |
| Severity | Informational / Lab Exercise |
| Environment | SOC Home Lab |
| SIEM | Splunk Enterprise |
| Affected Endpoint | CLIENT01 |
| Windows Event ID | 4625 |
| Investigation Status | Completed |

## 1. Executive Summary

During a controlled SOC home lab exercise, a failed Windows authentication attempt was generated on CLIENT01.

The resulting Windows Security Event ID 4625 was forwarded to Splunk Enterprise through the Splunk Universal Forwarder.

The event was located and reviewed using Splunk Search Processing Language (SPL).

This exercise demonstrated centralized security event collection and basic authentication-event investigation.

## 2. Detection

The following SPL search was used:

```spl
index=main EventCode=4625
| stats count by host Account_Name Source_Network_Address
| sort - count
```

The search identifies failed authentication events and summarizes them by endpoint, account, and source address.

## 3. Evidence Collected

| Evidence | Result |
|---|---|
| Windows Event ID 4625 | Successfully located |
| Endpoint | CLIENT01 |
| Splunk ingestion | Verified |
| Event forwarding | Verified |
| Authentication investigation | Completed |

## 4. Investigation Process

The following steps were performed:

1. Generated a controlled failed authentication event on CLIENT01.
2. Confirmed that Windows recorded the event.
3. Verified that the Splunk Universal Forwarder transmitted the event.
4. Opened Splunk Search and Reporting.
5. Executed the Event ID 4625 search.
6. Located and reviewed the resulting event.

## 5. MITRE ATT&CK Relevance

**Potential technique:** Brute Force (T1110)

Repeated failed authentication events can be relevant to brute-force investigations.

However, one failed login does not establish brute-force activity.

Additional evidence, including event frequency, source address, affected accounts, and surrounding activity, would be required to support that conclusion.

## 6. Findings

The lab successfully demonstrated that:

- Windows authentication failures can be collected centrally.
- Splunk can search and summarize authentication events.
- Endpoint telemetry can support security investigations.
- Event ID 4625 provides useful evidence for authentication monitoring.

No actual compromise or successful attack was established by this exercise.

## 7. Conclusion

The investigation validated the SOC lab's ability to collect and investigate Windows failed authentication events.

This exercise provided practical experience with SIEM monitoring, Windows Security logs, event forwarding, SPL searches, and basic security investigation documentation.

## 8. Recommended Next Steps

- Configure a threshold-based failed-login alert.
- Compare failed and successful authentication events.
- Investigate unusual source addresses and account activity.
- Develop additional detection searches.
- Perform controlled attack simulations in the isolated lab.

**Project:** Cybersecurity SOC Home Lab  
**Repository:** https://github.com/francisr94/Cybersecurity-SOC-HomeLab
