# Vulnerability Management Portfolio Lab

A hands-on vulnerability management lab built around Greenbone Vulnerability Management (GVM/OpenVAS), manual validation, targeted remediation, and authenticated re-scanning.

## Project objective

Demonstrate an end-to-end vulnerability management workflow:

1. Build and validate an isolated lab.
2. Map assets and establish a baseline.
3. Prioritize findings by severity and exposure.
4. Manually validate selected findings.
5. Apply targeted remediation.
6. Re-scan affected assets, including authenticated scans.
7. Measure the change and document residual risk.

## Lab scope

| Asset | Role | Scan context |
|---|---|---|
| 192.168.100.128 | DVWA / legacy Ubuntu web server | Unauthenticated |
| 192.168.100.129 | Windows 11 target | Authenticated SMB |
| 192.168.100.131 | Ubuntu target | Authenticated SSH |
| 192.168.100.132 | Kali / GVM management host | Scanner / management |

All testing was performed inside an isolated VMware lab.

## Key results

### Unauthenticated DVWA baseline → final

- Baseline displayed findings: **40**
  - Critical: 1
  - High: 2
  - Medium: 32
  - Low: 5
- Final displayed findings: **28**
  - Critical: 1
  - High: 1
  - Medium: 23
  - Low: 3
- Net change in displayed findings: **-12 (-30%)**

The reduction is a scan-result comparison, not a claim that every underlying vulnerability disappeared. The legacy DVWA host still carries significant residual risk.

### Authenticated Ubuntu

Baseline identified 3 Low findings:

- TCP timestamps
- Weak SSH MAC algorithms
- ICMP timestamp replies

After remediation, the final authenticated scan reported **0 displayed findings**.

### Authenticated Windows

Baseline identified 1 Low finding:

- TCP timestamps

After remediation and authenticated re-scan, the final report showed **0 displayed findings**.

## Selected remediation work

### DVWA / legacy web server

- Disabled explicit /server-status exposure.
- Disabled explicit /server-info exposure.
- Blocked /.svn and related Subversion metadata paths.
- Hardened the legacy TLS configuration as far as the old Apache/OpenSSL stack allowed.
- Preserved backups before web/TLS configuration changes.

### Ubuntu

- Disabled TCP timestamps using sysctl.
- Removed weak SSH MAC algorithms.
- Blocked ICMP timestamp requests.
- Persisted firewall rules with iptables-persistent.
- Revalidated with authenticated GVM and direct network checks.

### Windows

- Disabled RFC 1323 TCP timestamps.
- Enabled the SMB-In firewall rule and used a Private network profile as a prerequisite for authenticated scanning.
- Revalidated with authenticated GVM and packet-level evidence.

## Evidence

### Reports
- Final technical report PDF: reports/Vulnerability_Management_Portfolio_Lab_Final_Report.pdf
- Final DVWA scan report: reports/GVM_Final_DVWA_Unauthenticated.pdf
- Final Ubuntu authenticated scan: reports/GVM_Final_Ubuntu_Authenticated.pdf
- Final Windows authenticated scan: reports/GVM_Final_Windows_Authenticated.pdf

### Documentation
- docs/final-report.md
- docs/methodology.md
- docs/remediation.md
- docs/residual-risk.md
- evidence/scan-results.md
- evidence/scan-reports.md
- evidence/validation-checks.md
- evidence/limitations.md

### Architecture
- evidence/architecture.svg
- evidence/before-after.svg

Note: the PDFs in reports/ are portfolio-formatted copies derived from the verified scan/report contents. The original GVM export files remain the authoritative scanner evidence.

## Responsible use

This repository documents defensive vulnerability-management work performed against intentionally lab-based systems. Do not apply the procedures to systems you do not own or have authorization to test.