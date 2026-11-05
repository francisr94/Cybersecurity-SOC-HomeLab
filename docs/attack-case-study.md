# Attack & Detection Case Study

## Scenario

[Describe the scenario in 2-3 sentences: what technique you simulated, from where, and
against what target. Example: "This case study simulates an external attacker attempting to
gain initial access to a domain-joined Windows client via SMB brute-force, using the
MITRE ATT&CK technique T1110 (Brute Force)."]

**MITRE ATT&CK Technique(s):** [e.g. T1110 - Brute Force]

## Step 1: The Adversarial Vector

**Tooling:** Kali Linux, [Hydra / Atomic Red Team / etc.]

[Describe exactly what command you ran and why. Keep it factual — this is the "what happened"
section.]

```
[the exact command you executed]
```

![Attack terminal](../screenshots/04-attack-detection/01-attack-command.png)

## Step 2: SIEM Ingestion & Query

**Tooling:** Splunk Enterprise

Once the activity hit the domain controller / client, it was picked up by Sysmon and
forwarded to Splunk via the Universal Forwarder. The following search isolates the event:

```spl
[paste your exact SPL query here — see queries/ folder for the saved version]
```

**Key fields analyzed:**
- Event ID: [e.g. 4625 for failed logon, or Sysmon Event ID 1 for process creation]
- Source Network Address: [attacker IP]
- Account Name / Process Command Line / Parent Process: [as relevant]

![Splunk search results](../screenshots/04-attack-detection/02-splunk-query.png)

## Step 3: Remediation

**Control implemented:** [e.g. "pfSense firewall rule blocking the attacker IP on the WAN
interface" or "Account lockout policy GPO: lockout after 5 failed attempts in 10 minutes"]

[Describe the exact change you made and where.]

![Remediation config](../screenshots/04-attack-detection/03-remediation.png)

## Verification

[Describe how you re-ran the attack and confirmed it was blocked/detected differently. This
"before vs. after" comparison is the most important part of the write-up — it proves the
control actually works, not just that it exists.]

![Verification](../screenshots/04-attack-detection/04-verification.png)

## Lessons Learned

[1-2 sentences: what this exercise taught you about detection engineering, SIEM tuning, or
defense-in-depth. This is the section that shows judgment, not just execution.]
