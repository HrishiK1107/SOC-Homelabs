# MITRE ATT&CK Rule Mapping — Lab 2

## Custom Rules

| Rule ID | Level | ATT&CK ID | Technique | Tactic | Detection Method |
|---------|-------|-----------|-----------|--------|-----------------|
| 100020 | 12 | T1053.005 | Scheduled Task Creation | Persistence | commandLine regex: `schtasks.*\/create` |
| 100021 | 13 | T1055 | Process Injection | Defense Evasion | commandLine regex: `mavinject\|CreateThreadNative\|CreateRemoteThread` |
| 100022 | 10 | T1070.004 | File Deletion | Defense Evasion | commandLine regex: `cmd.*del\|Remove-Item.*-Force` |
| 100023 | 15 | T1003.001 | LSASS Memory Dump | Credential Access | commandLine regex: `comsvcs.*MiniDump\|rundll32.*comsvcs` |
| 100024 | 12 | T1105 | Ingress Tool Transfer | Command & Control | commandLine regex: `WebClient\|DownloadFile\|DownloadString\|IWR` |
| 100025 | 12 | T1136.001 | Local Account Creation | Persistence | commandLine regex: `net.*user.*\/add` |

## OOB Rules Referenced

| Rule ID | ATT&CK ID | Technique | Notes |
|---------|-----------|-----------|-------|
| 92031 | T1016, T1049 | Network Discovery | Fires on net.exe commands |
| 92032 | T1082, T1053.005 | System Discovery / Scheduled Task | Generic process execution |
| 92041 | T1547.001 | Registry Run Keys | Fires on reg.exe modifications |
| 92027 | T1055, T1003.001, T1105 | PowerShell techniques | Generic PowerShell execution only |
| 92052 | T1070.004 | File Deletion | Generic cmd.exe execution only |
| 60109 | T1136.001 | Local Account Creation | Fires on Security Event ID 4720 (requires auditpol fix) |

## Rule Severity Rationale
| Level | Meaning | Rules |
|-------|---------|-------|
| 15 | Critical | 100023 (LSASS dump — immediate response required) |
| 13 | High | 100021 (Process injection — high confidence malicious) |
| 12 | Medium-High | 100020, 100024, 100025 (Suspicious but context needed) |
| 10 | Medium | 100022 (File deletion — common in malware cleanup) |

## Additional Configuration
T1136.001 requires Windows audit policy change to capture Event ID 4720:
```powershell
auditpol /set /subcategory:"User Account Management" /success:enable /failure:enable
```