# Evidence and Interpretation Notes

## Scan-count caveat

The primary before/after metric is the number of findings displayed in the GVM report. It is not a count of unique CVEs, vulnerabilities, or risk-weighted exposure.

A change in scanner behavior, filtering, QoD, or plugin coverage can change displayed counts without representing a one-to-one remediation relationship.

## Legacy host caveat

The DVWA target intentionally remains legacy. The project therefore demonstrates targeted reduction and validation rather than full modernization.

## Authenticated scan caveat

Authenticated scans are stronger evidence of host configuration state than unauthenticated scans, but they still depend on credential validity, scan coverage, and the scanner's detection logic.

## Public-repository hygiene

The portfolio excludes:
- passwords
- private keys
- tokens
- personal credentials
- raw credential files

Lab IP addresses are RFC1918 addresses used only as environment identifiers.
