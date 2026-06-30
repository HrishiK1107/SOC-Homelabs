# Sample Analysis 06 — Simulated Phishing Attack (Self-Generated)

## Overview
Unlike samples 1000–1004, this sample was not sourced from the phishing dataset.
It was generated as part of Phase 6 of the Lab 3 pipeline to validate detection
coverage end-to-end — from attack simulation through SIEM visibility to alerting.

## Objective
Confirm that a simulated phishing delivery attempt, sent over the network rather
than analyzed as a static file, would be visible in Sysmon/Wazuh telemetry and
trigger a meaningful detection.

## Environment
Sender:    kali (192.168.234.129) — swaks v20240103.0
Receiver:  windows10-lab (192.168.234.140) — custom Python SMTP capture listener
(aiosmtpd, port 1025)
Detection: soc-core (192.168.234.141) — Wazuh manager, Sysmon Event ID 3

## Attack Simulation

Sent from Kali using swaks:

```bash
swaks --to victim@lab.local \
      --from "security@paypa1.com" \
      --server 192.168.234.140:1025 \
      --header "Subject: Urgent: Your account has been suspended" \
      --body "Click here to verify: http://192.168.234.129/phish"
```

## Captured Artifact

The Windows-side listener (`smtp_capture.py`) wrote the message to disk as a real
`.eml` file:
Date: Tue, 30 Jun 2026 12:31:47 +0530
To: victim@lab.local
From: security@paypa1.com
Subject: Urgent: Your account has been suspended
Message-Id: 20260630123147.102258@kali.Kali
X-Mailer: swaks v20240103.0 jetmore.org/john/code/swaks/
Click here to verify: http://192.168.234.129/phish

Notable: this header set is trivially identifiable as synthetic — no real
`Received:` chain (single network hop, Kali to Windows directly), generic
`From:` spoof with no supporting SPF/DKIM/DMARC infrastructure, and the
`X-Mailer` field openly reveals swaks. This is expected and intentional;
the goal was network/detection validation, not lure realism.

## Detection Validation

### Sysmon (local, Windows Event Viewer)
Event ID 3 (Network Connection) logged the inbound connection:
Image: C:\Users\soc-user\AppData\Local\Python\pythoncore-3.14-64\python.exe
Protocol: tcp
SourceIp: 192.168.234.129
SourcePort: 40060
DestinationIp: 192.168.234.140
DestinationPort: 1025

### Wazuh (initial check)
The event reached the Wazuh manager and was confirmed present in
`archives.json` (full event log, `logall_json` enabled), but did not
generate an alert — no existing rule matched inbound traffic on port 1025.

### Gap identified
Default Wazuh ruleset has no coverage for unexpected SMTP/mail-relay traffic
on non-standard ports. This represents a real detection gap: a test (or
real) phishing relay running on an alternate port would pass through
silently.

### Custom rule authored

Added to `local_rules.xml`:

```xml
<group name="local,sysmon,network,">
  <rule id="100026" level="10">
    <if_group>sysmon_event3</if_group>
    <field name="win.eventdata.destinationPort">^1025$</field>
    <description>Suspicious Inbound Connection on Non-Standard SMTP Port 1025 - Possible Phishing Simulation/Test Mail Relay (T1071.003)</description>
    <mitre>
      <id>T1071.003</id>
    </mitre>
    <group>phishing,network_connection,</group>
  </rule>
</group>
```

### Validation result
Resent the simulation after restarting the Wazuh manager. Rule 100026 fired
successfully — confirmed in `alerts.json` and the Wazuh dashboard, correctly
enriched with full Sysmon event data (source IP, destination port, process
image).

## Verdict
**Detection validated.** This sample closes the loop for Lab 3: a simulated
phishing delivery was generated, captured as a real artifact, and detected
via a custom Wazuh rule built specifically to cover the gap identified during
testing. Severity classified as Low — this is controlled internal test
traffic, not a genuine threat, but the rule logic (any inbound connection to
port 1025) would equally flag a real-world test mail relay or misconfigured
service.

## Indicators (Lab Infrastructure — Not External IOCs)
| Type | Value | Role | IOC Flag |
|------|-------|------|----------|
| ip | 192.168.234.129 | Kali — simulation source | Off (internal lab host) |
| port | 1025 | Listener port, non-standard SMTP | N/A |
| rule | Wazuh 100026 | Custom detection rule, T1071.003 | N/A |
