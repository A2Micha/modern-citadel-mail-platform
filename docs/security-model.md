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

The firewall opens ports 25, 80, 443, 465, 587 and 993, plus the VPN port. SSH and administrative access are reached through the VPN.

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

Active jails:

- sshd
- postfix-smtp
- postfix-msasubmission
- citadel-auth
- perma-ban

Citadel logs to the systemd journal. The `citadel-auth` jail uses the systemd backend and reads `citserver` messages with `SYSLOG_IDENTIFIER=citserver`. `journalflags=1` is set because `citserver` runs as the `citadel` user. The jail needs the Debian package `python3-systemd`.

The jail matches Citadel's failed-password message, for IPv4 and IPv6:

```text
CtdlTryPassword: bad password specified for <user> ... Remote <host / IP>
```

Five failures in 10 minutes produce a ban of 1 hour on ports 80, 443, 465, 587 and 993. The jail counts failed passwords. A connection by itself does not create a ban. Localhost and the admin VPN are ignored.

The earlier Citadel jails `citadel-msasubmission`, `smtp-citadel`, `smtp-submission` and `webcit` are retired.

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

MTA-STS for motobike.fun is published in enforce mode, with `mx` motobike.fun and `max_age` 86400. WebCit serves the policy from `/usr/local/webcit/static/.well-known/mta-sts.txt` at `https://mta-sts.motobike.fun/.well-known/mta-sts.txt`.

Citadel DKIM-signs outbound mail with selector `dvbbt`. Rspamd ARC-signs the outbound relay with selector `arc2026`. DMARC is published as `p=reject` and `sp=reject`. SPF authorizes `65.109.224.37`.

Since 2026-10-18, Citadel compares passwords case-sensitively (`c_case_sensitive_passwords=1`).

These technologies improve:

- sender authenticity
- delivery reputation
- transport security
- anti-spoofing protection

---

# Let's Encrypt

Certificates come from Let's Encrypt through certbot, using the webroot authenticator.

The webroot is `/usr/local/webcit/static`. WebCit serves `/.well-known/acme-challenge` on port 80, so issuance and renewal run while the services stay up. `certbot.timer` renews certificates automatically.

The Citadel and WebCit certificate is used on ports 443, 465, 587 and 993. It covers:

- motobike.fun
- mail.motobike.fun
- smtp.motobike.fun
- mta-sts.motobike.fun
- xmpp.motobike.fun
- mail.g00r00.com
- mail.vibedullis.com

`/usr/local/citadel/keys/citadel.cer` and `citadel.key` are symlinks into `/etc/letsencrypt/live/`.

Postfix on port 25 uses its own Let's Encrypt certificate.

A pre hook makes sure the webroot `.well-known` directory exists. A deploy hook checks the Citadel key symlinks and restarts `citadel`, `webcit-http` and `webcit-https`. The same deploy hook reloads Postfix when the Postfix certificate renews.

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

Rspamd uses Redis for Bayesian statistics and the ARF suppression list. There is intentionally no global Redis block.

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

The production server is covered by daily Hetzner backups.

The backup design focuses on:

- a daily recovery point
- reliability
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
