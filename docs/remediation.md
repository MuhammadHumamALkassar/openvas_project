# Remediation Record

## 1. DVWA legacy web server

### Server-status

Observed before remediation:
- GET /server-status returned HTTP 200 and Apache status content.

Change:
- Disabled the explicit `/server-status` location in the Apache configuration.

Validation:
- Direct request returned HTTP 404 after the change.

### Server-info

Observed before remediation:
- GET /server-info returned HTTP 200 and Apache server information.

Change:
- Disabled the explicit `/server-info` location.

Validation:
- Direct request returned HTTP 404 after the change.

### Subversion metadata

Observed before remediation:
- `/.svn/entries`
- `/dvwa/.svn/entries`

returned HTTP 200 with SVN metadata.

Change:

```apache
<LocationMatch "(?i)^/.*\.svn(?:/.*)?$">
    Order allow,deny
    Deny from all
</LocationMatch>
```

Validation:
- Both tested paths returned HTTP 403 after the change.

### TLS hardening

Backup:
- `/opt/lampp/etc.backup-before-tls`

Configuration changes:

```apache
SSLProtocol all -SSLv2 -SSLv3
SSLCipherSuite AES256-SHA:AES128-SHA
```

`apachectl -t` returned Syntax OK before restart.

Important limitation:
- The legacy Apache/OpenSSL implementation still exposed TLS 1.0 behavior and other legacy weaknesses after the change. This is documented as partial hardening, not full TLS remediation.

## 2. Ubuntu target

### TCP timestamps

Before:
- `net.ipv4.tcp_timestamps` was enabled.

Change:

```text
net.ipv4.tcp_timestamps = 0
```

Applied with:

```text
sudo sysctl --system
```

Final validation:
- sysctl value = 0.

### SSH weak MAC algorithms

The baseline identified weak UMAC-64 variants.

After hardening, direct SSH algorithm enumeration showed only:

```text
hmac-sha2-512-etm@openssh.com
hmac-sha2-256-etm@openssh.com
hmac-sha2-512
hmac-sha2-256
```

No UMAC-64 variants were shown.

### ICMP timestamp replies

Before:
- Nping received 3/3 ICMP timestamp replies.

Change:

```text
iptables -A INPUT -p icmp --icmp-type timestamp-request -j DROP
```

After:
- Nping received 0/3 replies (100% packet loss).

The rule was persisted using iptables-persistent/netfilter-persistent.

## 3. Windows target

### TCP timestamps

Before:
- Windows reported RFC 1323 timestamps as allowed.

Change:

```text
netsh int tcp set global timestamps=disabled
```

After:
- Windows reported RFC 1323 timestamps as disabled.

Packet-level validation:
- SYN/ACK behavior was inspected with Nmap packet tracing and no displayed TCP timestamp option was observed in the SYN/ACK.

Important interpretation:
- The GVM plugin's wording notes that modern Windows behavior may not be fully eliminated in every context; final authenticated scanning is therefore the primary verification result.
