# IOC Master List — Lab 3 Phishing Analysis

## IP Addresses
| IP | Sample | Role |
|----|--------|------|
| 89.144.9.87 | sample-1001 | Sending IP |
| 89.144.44.2 | sample-1002 | Sending IP |
| 103.225.77.255 | sample-1001/1002 | Fake signin IP (body) |
| 198.61.254.55 | sample-1003 | Mailgun sending IP |
| 159.27.24.86 | sample-1004 | Relay sending IP |
| 147.78.103.9 | sample-1004 | Origin IP |
| 209.85.160.178 | sample-1000 | Google SMTP relay |
| 20.97.213.223 | sample-1000 | Attacker origin (Azure) |

## Domains
| Domain | Sample | Role |
|--------|--------|------|
| access-accsecurity.com | 1001/1002 | Typosquatted Microsoft |
| nonkfrgr.co.uk | 1001 | Throwaway sending domain |
| thcultarfdes.co.uk | 1002 | Throwaway sending domain |
| thebandalisty.com | 1001/1002 | Tracking pixel host |
| mg.areafellowship.com | 1003 | Abused Mailgun domain |
| mail123-ripple.net | 1003 | Crypto drainer domain |
| zhanlingol.com | 1004 | Mail relay |
| cyber.net.pk | 1004 | Abused From domain |

## Email Addresses
| Address | Sample | Role |
|---------|--------|------|
| solutionteamrecognizd02@gmail.com | 1001/1002 | Attacker Reply-To |
| no-reply@access-accsecurity.com | 1001/1002 | Spoofed sender |
| bounce@nonkfrgr.co.uk | 1001 | Return-Path |
| bounce@thcultarfdes.co.uk | 1002 | Return-Path |
| coindesk@mg.areafellowship.com | 1003 | Spoofed sender |
| ghulammustafa@cyber.net.pk | 1004 | Spoofed From |
| philipffredrick3690@gmail.com | 1004 | Attacker Reply-To |
| prestonconstance587@gmail.com | 1000 | Attacker Gmail |

## URLs
| URL | Sample | Role |
|-----|--------|------|
| http://thebandalisty.com/track/... | 1001/1002 | Tracking pixel |
| https://mail123-ripple.net/... | 1003 | Crypto drainer |

## Attachments
| Filename | Sample | Type |
|----------|--------|------|
| csWuYjyqO2IR.pdf | 1000 | Malicious PDF |
