# Scan Results Register

## Unauthenticated baseline

Scope:
- 192.168.100.128
- 192.168.100.129
- 192.168.100.131

Displayed result summary:

| Host | Critical | High | Medium | Low |
|---|---:|---:|---:|---:|
| 192.168.100.128 | 1 | 2 | 32 | 4 |
| 192.168.100.129 | 0 | 0 | 0 | 0 |
| 192.168.100.131 | 0 | 0 | 0 | 1 |
| **Total** | **1** | **2** | **32** | **5** |

Raw results before filtering: 601.

## Ubuntu authenticated baseline

Host:
- 192.168.100.131

Authentication:
- SSH success

Displayed findings: 3 Low.

Findings:
1. TCP timestamps
2. Weak SSH MAC algorithms
3. ICMP timestamp reply

## Windows authenticated baseline

Host:
- 192.168.100.129

Authentication:
- SMB success

Displayed findings: 1 Low.

Finding:
- TCP timestamps

## Unauthenticated final

Authoritative final report:
- Scan window: 2026-09-28 09:54:08–10:06:50 UTC
- Host: 192.168.100.128 carries the remaining displayed findings
- Total displayed: 28
- Critical: 1
- High: 1
- Medium: 23
- Low: 3
- Raw before filtering: 591

## Ubuntu authenticated final

- Scan window: 2026-09-28 09:45:58–09:51:59 UTC
- SSH authentication: Success
- Displayed findings: 0
- Critical: 0
- High: 0
- Medium: 0
- Low: 0

## Windows authenticated final

- Scan window: 2026-09-28 09:16:27–09:37:01 UTC
- SMB authentication: Success
- Displayed findings: 0
- Critical: 0
- High: 0
- Medium: 0
- Low: 0
