# Security Model

## Overview

The Modern Citadel Mail Platform uses a layered security architecture designed around the principle of reducing attack surface while maintaining operational simplicity.

Instead of relying on large centralized security appliances or complex orchestration frameworks, the system combines lightweight Unix security components working together in multiple layers.

The security model focuses on:

- minimizing exposed services
- restricting administrative access
- filtering unwanted traffic early
- reducing automated abuse
- enforcing modern mail authentication standards
- maintaining transparent infrastructure

---

# Security Layers

```text
Internet
    |
Hetzner Firewall
    |
UFW
    |
nftables GeoIP Filtering
    |
Fail2ban
    |
Application Services
```

---

# Hetzner Upstream Firewall

The first protection layer is provided by the Hetzner upstream firewall.

This firewall filters unwanted traffic before packets reach the virtual server itself.

Advantages include:

- early traffic filtering
- reduced attack exposure
- reduced unnecessary load
- additional isolation layer

---

# UFW Host Firewall

UFW is used as the local host firewall.

Only explicitly required services are reachable.

The firewall policy follows a restrictive approach:

- deny by default
- allow only required ports
- reduce public exposure

---

# nftables GeoIP Filtering

Geo-based filtering is implemented using nftables.

This layer restricts access to selected services based on geographic origin.

The filtering primarily protects:

- IMAP
- POP3
- SMTPS
- submission services

SMTP port 25 remains globally reachable for standard mail delivery.

Advantages:

- reduction of automated abuse traffic
- reduced brute-force attempts
- smaller external attack surface
- reduced unnecessary authentication requests

---

# WireGuard Administrative Access

Administrative access is restricted through WireGuard VPN.

SSH is not exposed publicly to the Internet.

This approach significantly reduces:

- automated SSH scanning
- brute-force attacks
- exploit attempts against SSH services

Only authenticated VPN clients can access management interfaces.

---

# Fail2ban

Fail2ban monitors service logs and dynamically blocks abusive IP addresses.

The system protects services including:

- Postfix
- IMAP
- POP3
- Web services
- authentication services

Fail2ban operates together with nftables and firewall rules to automatically react to suspicious behavior.

---

# Mail Security Standards

The platform implements modern mail authentication and security standards including:

- SPF
- DKIM
- DMARC
- ARC
- BIMI
- MTA-STS
- TLS-RPT
- DNSSEC
- DANE/TLSA preparation

These technologies improve:

- sender authenticity
- delivery reputation
- transport security
- anti-spoofing protection

---

# Rspamd Security Functions

Rspamd provides multiple protection and reputation functions:

- spam filtering
- Bayesian classification
- reputation scoring
- greylisting
- ARC validation
- DMARC evaluation
- abuse handling
- heuristic analysis

Rspamd operates with Redis as lightweight backend storage.

---

# DNS Security

DNS resolution is handled locally using Unbound.

Advantages:

- independent recursive resolution
- DNSSEC validation
- reduced dependency on external resolvers
- faster local DNS lookups

DNSSEC is enabled for signed domains.

---

# Backup Security

Backups are performed using BorgBackup within the Hetzner infrastructure.

The backup design focuses on:

- incremental operation
- reliability
- low storage overhead
- recovery simplicity

---

# Monitoring

Infrastructure monitoring currently focuses on operational simplicity.

Monitoring methods include:

- Hetzner Cloud dashboard
- service health observation
- Rspamd statistics
- system load monitoring
- mail queue observation

The system intentionally avoids unnecessary monitoring complexity.

---

# Security Philosophy

The overall security philosophy of the platform is based on:

- multiple small security layers
- reduction of exposed attack surface
- predictable Unix service behavior
- operational transparency
- stable long-term maintainability

The project demonstrates that modern secure communication infrastructure can be achieved without excessive architectural complexity.
