# Residual Risk

The final scan is intentionally documented with unresolved findings. This section prevents the portfolio from presenting a partial remediation as a fully secure state.

## Legacy operating system

The DVWA host remains on Ubuntu 10.04.1 LTS, which is end-of-life. The GVM report rates this as Critical.

This is the dominant architectural risk because application-level hardening cannot compensate for an unsupported operating system indefinitely.

## Legacy cryptography and TLS

The final DVWA scan still reports legacy protocol/cryptographic exposure, including:

- OpenSSL CCS vulnerability
- deprecated TLS 1.0
- weak SSH algorithms
- weak/short RSA certificate material
- expired certificate findings

The attempted TLS hardening improved the configuration but did not eliminate all weaknesses because the underlying software stack is obsolete.

## Web/application layer

The final report still contains application-level issues involving:

- phpMyAdmin XSS
- missing cookie security attributes
- cleartext authentication transport
- other legacy web-service weaknesses

## Network services

FTP cleartext authentication and other legacy service behavior remain relevant.

## Risk interpretation

The project demonstrates that remediation of individual findings can be successful while systemic risk remains high when the underlying platform is obsolete.

A production remediation plan would therefore consider platform replacement/upgrade, service retirement, TLS stack modernization, certificate replacement, and application hardening—not only individual plugin findings.
