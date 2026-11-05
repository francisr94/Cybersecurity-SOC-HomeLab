> ⚠️ **THIS IS AN EXAMPLE, NOT YOUR SUBMISSION.**
> Every value below (IPs, hostnames, output, timestamps) is made up to show you the
> format and level of detail expected. Delete this file once you've used it as a
> reference — your real write-up goes in `attack-case-study.md`, built from your own
> screenshots and your own Splunk output.

# Attack & Detection Case Study (EXAMPLE)

## Scenario

This case study simulates an external attacker attempting to gain initial access to a
domain-joined Windows client via SMB brute-force, using MITRE ATT&CK technique
**T1110 (Brute Force)**. The attack originates from a Kali Linux VM on an isolated
"Attacker" network segment and targets a Windows 10 client joined to the `corp.local`
domain.

**MITRE ATT&CK Technique:** T1110.001 – Brute Force: Password Guessing

## Step 1: The Adversarial Vector

**Tooling:** Kali Linux, Hydra

From the Kali VM (example IP `10.10.20.15`), I ran a brute-force attempt against SMB on
the Windows client (example IP `192.168.10.55`) using a small test wordlist of 20 common
passwords against a known test account, `jsmith`:

```
hydra -l jsmith -P /usr/share/wordlists/small_test_list.txt smb://192.168.10.55
```

Example output:
```
[DATA] max 4 tasks per 1 server, overall 4 tasks, 20 login tries
[ATTEMPT] target 192.168.10.55 - login "jsmith" - pass "password1" - 1 of 20
[ATTEMPT] target 192.168.10.55 - login "jsmith" - pass "welcome1" - 2 of 20
...
[ERROR] could not connect - account locked out
```

*(Your real screenshot here would show your actual terminal output —
`screenshots/04-attack-detection/01-attack-command.png`)*

## Step 2: SIEM Ingestion & Query

**Tooling:** Splunk Enterprise

The failed logon attempts generated Windows Security Event ID 4625 on the domain
controller, forwarded via the Splunk Universal Forwarder. I isolated the activity with:

```spl
index=* EventCode=4625
| bucket _time span=5m
| stats count by _time, Source_Network_Address, Account_Name
| where count > 5
```

Example result table:

| _time | Source_Network_Address | Account_Name | count |
|---|---|---|---|
| 2026-10-20 14:32:00 | 10.10.20.15 | jsmith | 14 |

This confirms 14 failed login attempts against the `jsmith` account from the attacker
segment within a 5-minute window — well above normal user error (1-2 typos) and a clear
brute-force signature.

**Key fields analyzed:**
- Event ID: 4625 (An account failed to log on)
- Source Network Address: 10.10.20.15 (attacker/Kali VM)
- Account Name: jsmith (targeted test account)

*(Your real screenshot here: `screenshots/04-attack-detection/02-splunk-query.png`)*

## Step 3: Remediation

**Control implemented:** Account lockout policy via Group Policy

On the domain controller, I edited the "Baseline Security" GPO
(Computer Configuration → Policies → Windows Settings → Security Settings →
Account Policies → Account Lockout Policy):

- Account lockout threshold: 5 invalid attempts
- Account lockout duration: 30 minutes
- Reset account lockout counter after: 15 minutes

Ran `gpupdate /force` on the client to confirm the policy applied immediately.

*(Your real screenshot here: `screenshots/04-attack-detection/03-remediation.png`)*

## Verification

Re-running the same Hydra command after the GPO was applied, the account locked out
after the 5th attempt instead of allowing all 20 guesses, and Splunk showed a follow-up
**Event ID 4740 (account locked out)** immediately after the 5th failed attempt —
confirming the control stopped the brute-force attempt well before exhausting even a
small wordlist.

```spl
index=* EventCode=4740
| table _time, Target_Account_Name, Caller_Computer_Name
```

*(Your real screenshot here: `screenshots/04-attack-detection/04-verification.png`)*

## Lessons Learned

This exercise showed how a basic account lockout policy — something easy to overlook in
a lab environment — directly closes the gap exposed by unlimited authentication attempts.
It also reinforced the value of correlating multiple event types (4625 and 4740 together)
rather than relying on a single log source to confirm whether a control is actually
working, not just configured.
