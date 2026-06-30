# Sample Analysis 04 — Advance Fee Fraud (419 Scam)

## Source File
sample-1004.eml

## Attack Type
Nigerian advance fee fraud — fake philanthropist donation lure

## Header Analysis

| Field | Value |
|-------|-------|
| From | "Philip Fredrick" <ghulammustafa@cyber.net.pk> |
| Reply-To | philipffredrick3690@gmail.com |
| Return-Path | ghulammustafa@cyber.net.pk |
| Sending IP | 159.27.24.86 (zhanlingol.com relay) |
| Origin IP | 147.78.103.9 |
| SPF | SOFTFAIL |
| DKIM | NONE |
| DMARC | FAIL (quarantine) |
| SCL | 7 |

## Red Flags
- From name vs email address mismatch
- Reply-To redirects to Gmail (attacker collection)
- Sent via third-party relay (zhanlingol.com)
- Fake X-Mailer (Outlook Express 2001)
- Mass blast (Undisclosed recipients)
- Subject prefix "86RE:" fakes reply chain

## IOCs

### IPs
| IP | Role |
|----|------|
| 159.27.24.86 | Relay sending IP |
| 147.78.103.9 | Actual origin IP |

### Domains
| Domain | Role |
|--------|------|
| zhanlingol.com | Mail relay |
| cyber.net.pk | Abused From domain |

### Email Addresses
| Address | Role |
|---------|------|
| ghulammustafa@cyber.net.pk | Spoofed From |
| philipffredrick3690@gmail.com | Attacker Reply-To |

## Verdict
- Classification: MALICIOUS
- Severity: MEDIUM
- Sophistication: LOW
