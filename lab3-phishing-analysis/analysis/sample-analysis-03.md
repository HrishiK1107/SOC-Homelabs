# Sample Analysis 03 — Crypto Drainer (CoinDesk Impersonation)

## Source File
sample-1003.eml

## Attack Type
Crypto drainer — CoinDesk brand impersonation, Ripple XRP lure

## Header Analysis

| Field | Value |
|-------|-------|
| From | CoinDesk <coindesk@mg.areafellowship.com> |
| Sender | coindesk@mg.areafellowship.com |
| Sending IP | 198.61.254.55 (Mailgun) |
| SPF | PASS (Mailgun authorized) |
| DKIM | PASS (mg.areafellowship.com) |
| DMARC | BESTGUESSPASS |
| SCL | 9 (Microsoft max spam score) |

## Why Auth Passes But Email Is Malicious
Attacker abused a Mailgun account under areafellowship.com
(a legitimate organization's domain) to send phishing via
Mailgun's authorized infrastructure. SPF/DKIM pass because
Mailgun is a legitimate ESP — the abuse is at the account level.

## IOCs

### IPs
| IP | Role |
|----|------|
| 198.61.254.55 | Mailgun sending IP (abused) |

### Domains
| Domain | Role |
|--------|------|
| mg.areafellowship.com | Abused Mailgun sender domain |
| mail123-ripple.net | Primary phishing/drainer domain |

### Victim Tracking
URL path hex decodes to: rodrigo-f-p@hotmail.com
Attacker encodes victim email in every link — confirms open/click per victim.

### URLs
| URL | Role |
|-----|------|
| https://mail123-ripple.net/726f647269676f2d... | Crypto drainer landing page |
| https://mail123-ripple.net/...gif | Tracking pixel |

## Verdict
- Classification: MALICIOUS
- Severity: HIGH
- Sophistication: HIGH — auth bypass via legitimate ESP abuse
- MITRE: T1566.002 (Spearphishing Link)
