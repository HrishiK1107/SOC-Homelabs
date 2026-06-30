# Sample Analysis 05 — Brazilian Tax Refund Phishing (IRPF)

## Source File
sample-1000.eml

## Attack Type
Brazilian IRPF (tax refund) phishing with malicious PDF attachment

## Header Analysis

| Field | Value |
|-------|-------|
| From | [BB] Seu saldo foi liberado <prestonconstance587@gmail.com> |
| Subject | Liberação de IRPF (base64 encoded) |
| Sending IP | 209.85.160.178 (Google SMTP) |
| Origin IP | 20.97.213.223 (Azure) |
| SPF | PASS |
| DKIM | PASS (gmail.com) |
| DMARC | PASS |
| SCL | 1 (nearly evaded detection) |

## Evasion Techniques
- From and Subject base64 encoded to evade content filters
- Sent via legitimate Google SMTP infrastructure
- Attacker used Azure VM (20.97.213.223) as origin

## IOCs

### IPs
| IP | Role |
|----|------|
| 209.85.160.178 | Google SMTP relay (abused) |
| 20.97.213.223 | Attacker origin (Azure cloud) |

### Email Addresses
| Address | Role |
|---------|------|
| prestonconstance587@gmail.com | Attacker Gmail |

### Attachments
| Filename | Type | Notes |
|----------|------|-------|
| csWuYjyqO2IR.pdf | PDF | Malicious — random filename pattern |

## Verdict
- Classification: MALICIOUS
- Severity: HIGH
- Sophistication: HIGH
- Target: Brazilian Portuguese speakers
