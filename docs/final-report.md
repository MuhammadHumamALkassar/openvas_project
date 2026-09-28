# Vulnerability Management Portfolio Lab — Final Technical Report

## 1. Executive summary

This project demonstrates a complete vulnerability-management cycle using GVM/OpenVAS in an isolated VMware lab. The workflow covered asset discovery, baseline vulnerability scanning, manual validation of selected findings, remediation, authenticated verification, and final reporting.

The most important outcome was not simply the number of findings. The project demonstrates the operational loop required in real vulnerability management: identify → validate → remediate → verify → document residual risk.

## 2. Environment

The management host was a Kali Linux VM running the GVM stack as normal system services:

- gsad
- gvmd
- ospd-openvas
- PostgreSQL
- Redis/OpenVAS

Registered scanners included OpenVAS and CVE data sources.

### Assets

- **192.168.100.128** — legacy DVWA web application host
- **192.168.100.129** — Windows 11 target
- **192.168.100.131** — Ubuntu target
- **192.168.100.132** — GVM management host

The environment was isolated and intended for defensive testing.

## 3. Methodology

### Phase 1 — Baseline

A baseline unauthenticated scan covered the three target hosts. The baseline displayed 40 findings:

| Severity | Count |
|---|---:|
| Critical | 1 |
| High | 2 |
| Medium | 32 |
| Low | 5 |
| **Total** | **40** |

Raw results before filtering were higher; the project tracks the displayed severity counts for portfolio-level before/after measurement.

### Phase 2 — Risk-focused validation

Selected findings were manually verified using direct requests, OpenSSL/Nmap checks, system configuration inspection, and packet-level tests.

Validation was used to distinguish scanner output from observable system behavior.

### Phase 3 — Remediation

Remediation was limited to changes that were appropriate for this deliberately legacy lab. The DVWA host was not modernized wholesale because its outdated platform is part of the test scenario.

### Phase 4 — Authenticated verification

The Ubuntu and Windows targets were verified through authenticated GVM scans after hardening changes.

### Phase 5 — Final unauthenticated verification

A later final unauthenticated scan of the three-host scope was used as the authoritative final DVWA result set.

## 4. Final results

### DVWA final scan

The authoritative final unauthenticated report showed:

| Severity | Count |
|---|---:|
| Critical | 1 |
| High | 1 |
| Medium | 23 |
| Low | 3 |
| **Total** | **28** |

Compared with the baseline, the displayed finding count decreased from 40 to 28, a reduction of 12 findings (30%).

### Ubuntu final authenticated scan

The final authenticated scan reported:

- SSH authentication: Success
- User context: test
- Critical: 0
- High: 0
- Medium: 0
- Low: 0
- Displayed findings: **0**

### Windows final authenticated scan

The final authenticated scan reported:

- SMB authentication: Success
- User context: openvas-test
- Critical: 0
- High: 0
- Medium: 0
- Low: 0
- Displayed findings: **0**

## 5. Important findings remaining on the legacy DVWA host

The final DVWA report still contained significant residual exposure, including:

- End-of-life Ubuntu 10.04 operating system (Critical)
- OpenSSL CCS man-in-the-middle exposure (High)
- Legacy SSH key-exchange and cipher algorithms
- Deprecated TLS 1.0 behavior in the legacy stack
- Weak 1024-bit RSA certificate
- Expired / weakly signed certificate material
- Missing Secure and HttpOnly cookie attributes
- Multiple phpMyAdmin XSS findings
- Cleartext password transmission over HTTP
- FTP cleartext login exposure
- Additional legacy web/application weaknesses

These results are why the project outcome is documented as measurable improvement rather than full remediation.

## 6. Change management notes

Configuration changes were preceded by backups on the legacy Apache/XAMPP host.

For the Ubuntu host, persistence was explicitly validated for both sysctl configuration and firewall rules.

For the Windows host, SMB reachability was restored as a prerequisite to authenticated scanning; this was a connectivity requirement and not itself a vulnerability finding.

## 7. Evidence model

The portfolio intentionally separates:

- scanner-reported evidence,
- manual validation evidence,
- remediation changes,
- and final verification.

This makes the project auditable and prevents treating a single scanner result as proof of remediation.

## 8. Conclusion

The project demonstrates a practical vulnerability-management lifecycle with a measurable before/after comparison, authenticated verification on two operating-system targets, and explicit documentation of residual risk on a legacy application host.
