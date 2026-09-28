# Manual Validation Checks

## DVWA

### HTTP exposure checks

Before remediation:
- `/server-status` → HTTP 200
- `/server-info` → HTTP 200
- `.svn` metadata paths → HTTP 200

After remediation:
- `/server-status` → HTTP 404
- `/server-info` → HTTP 404
- tested `.svn` paths → HTTP 403

### TLS checks

Before:
- legacy TLS/SSL protocols and weak ciphers were observable with direct testing.
- certificate material was expired and based on a 1024-bit RSA key.

After:
- Apache configuration syntax was valid.
- SSLv2/SSLv3 were explicitly removed from the configuration.
- the legacy implementation still exposed TLS 1.0, so remediation remained partial.

## Ubuntu

### TCP timestamps

Before:
- enabled

After:
- sysctl = 0

### SSH MACs

Before:
- weak UMAC-64 variants were present.

After:
- only SHA-2 HMAC/ETM algorithms listed in the validation output.

### ICMP timestamps

Before:
- 3/3 timestamp replies

After:
- 0/3 replies after firewall rule

## Windows

### TCP timestamps

Before:
- RFC 1323 timestamps: allowed

After:
- RFC 1323 timestamps: disabled

Packet-level check:
- inspected SYN/ACK with Nmap `--packet-trace`; no displayed timestamp option in the observed SYN/ACK.
