# Sample Analysis 02 — Microsoft Account Spoofing (Campaign Variant)

## Source File
sample-1002.eml

## Attack Type
Same campaign as Sample-01 — rotating infrastructure variant

## Header Analysis

| Field | Value |
|-------|-------|
| From | Microsoft account team <no-reply@access-accsecurity.com> |
| Return-Path | bounce@thcultarfdes.co.uk |
| Reply-To | solutionteamrecognizd02@gmail.com |
| X-Sender-IP | 89.144.44.2 |
| SPF | NONE (fail) |
| DKIM | NONE (unsigned) |
| DMARC | PERMERROR |

## Campaign Link to Sample-01
| Indicator | Sample-01 | Sample-02 |
|-----------|-----------|-----------|
| From domain | access-accsecurity.com | access-accsecurity.com |
| Reply-To | solutionteamrecognizd02@gmail.com | solutionteamrecognizd02@gmail.com |
| Body IP | 103.225.77.255 | 103.225.77.255 |
| Tracker host | thebandalisty.com | thebandalisty.com |
| Sending IP | 89.144.9.87 | 89.144.44.2 |
| Return-Path domain | nonkfrgr.co.uk | thcultarfdes.co.uk |

## New IOCs
| Type | Value | Role |
|------|-------|------|
| IP | 89.144.44.2 | Sending IP |
| Domain | thcultarfdes.co.uk | Return-Path domain |

## Verdict
- Classification: MALICIOUS
- Severity: HIGH
- Same threat actor as Sample-01, rotating infrastructure
