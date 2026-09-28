# Methodology

## Assessment model

The lab follows a repeatable cycle:

```text
Asset inventory
    ↓
Baseline scan
    ↓
Prioritize and validate
    ↓
Remediate selected findings
    ↓
Authenticated / direct verification
    ↓
Final scan
    ↓
Document residual risk
```

## Scan types

### Unauthenticated scanning

Used for the three-host baseline and final scope comparison. This represents an external perspective of exposed services without privileged host credentials.

### Authenticated scanning

Used for:

- Ubuntu via SSH
- Windows via SMB

Authenticated scanning provides additional host-level visibility and is particularly useful for validating configuration-oriented issues.

## Validation principles

A scanner result was treated as a lead for investigation rather than automatically assumed to be proof of a live condition.

Examples:

- HTTP paths were checked directly.
- TLS protocol/cipher behavior was inspected with Nmap/OpenSSL.
- SSH algorithms were enumerated directly.
- TCP timestamp behavior was checked at the host and packet level.
- ICMP timestamp behavior was tested before and after the firewall change.

## Measurement

The portfolio uses the **displayed finding counts** in the GVM reports for the primary before/after comparison.

For the DVWA host:

- Baseline = 40 displayed findings
- Final = 28 displayed findings
- Absolute change = -12
- Relative change = 30% reduction

This metric should not be interpreted as a universal security score.

## Evidence handling

Raw reports may contain environment-specific details. The public portfolio therefore focuses on sanitized summaries and reproducible methodology rather than credentials or secrets.
