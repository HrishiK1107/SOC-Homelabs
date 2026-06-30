# Sample Analysis 01 — Microsoft Account Spoofing

## Source File
sample-1001.eml

## Attack Type
Microsoft account spoofing — credential harvesting via fake security alert

## Header Analysis

| Field | Value |
|-------|-------|
| From | Microsoft account team <no-reply@access-accsecurity.com> |
| Return-Path | bounce@nonkfrgr.co.uk |
| Reply-To | solutionteamrecognizd02@gmail.com |
| X-Sender-IP | 89.144.9.87 |
| SPF | NONE (fail) |
| DKIM | NONE (unsigned) |
| DMARC | PERMERROR |

## IOCs

### IP Addresses
| IP | Role |
|----|------|
| 89.144.9.87 | Originating sender IP |
| 103.225.77.255 | Fake signin IP (body) |

### Domains
| Domain | Role |
|--------|------|
| access-accsecurity.com | Typosquatted Microsoft domain |
| nonkfrgr.co.uk | Sending/Return-Path domain |
| thebandalisty.com | Tracking pixel host |

### Email Addresses
| Address | Role |
|---------|------|
| no-reply@access-accsecurity.com | Spoofed sender |
| solutionteamrecognizd02@gmail.com | Attacker collection address |
| bounce@nonkfrgr.co.uk | Return-Path |

## Verdict
- Classification: MALICIOUS
- Severity: HIGH
- Confidence: HIGH
