# Platform Architecture

## Overview

The Modern Citadel Mail Platform is intentionally designed around classic Unix principles and lightweight service separation.

Instead of using large container orchestration systems or heavyweight groupware stacks, the platform relies on small specialized services working together in a transparent and predictable way.

The architecture focuses on:

- low complexity
- long-term maintainability
- low resource consumption
- modern mail security standards
- stable self-hosted communication

---

# High-Level Architecture

```text
                        Internet
                            |
                    Hetzner Firewall
                            |
                          UFW
                            |
                     nftables GeoIP
                            |
          +-----------------+-----------------+
          |                                   |
       Postfix                            WireGuard
          |                                   |
       Fail2ban                          SSH Access
          |
       Rspamd
          |
    +-----+-----+
    |           |
  Redis      Unbound
    |
 Citadel
    |
  WebCit

        Hetzner Backups
```

---

# Component Description

## Hetzner Firewall

The Hetzner upstream firewall filters unwanted traffic before it reaches the server infrastructure.

It opens ports 25, 80, 443, 465, 587 and 993, plus the VPN port. SSH and administrative access stay on the VPN.

This provides the first external security layer.

---

## UFW

UFW provides local host-based firewall filtering.

Only required services are exposed publicly.

---

## WireGuard

Administrative access is restricted to WireGuard VPN only.

SSH is not publicly reachable.

This significantly reduces the external attack surface.

---

## Postfix

Postfix acts as the SMTP frontend for incoming mail and as the outbound relay for Citadel.

Responsibilities include:

- SMTP on port 25
- recipient verification (`reject_unverified_recipient`, `550 5.1.1`)
- the localhost relay `postfix/citadel-out` on `127.0.0.1:10025`
- mail routing
- TLS on the public listener
- integration with Rspamd
- delivery to the unfiltered LMTP socket

---

## Rspamd

Rspamd 3.4 handles:

- spam filtering
- SPF validation
- DKIM verification
- DMARC validation
- ARC signing on the outbound relay (selector `arc2026`)
- greylisting for inbound mail
- reputation analysis
- ARF recipient suppression
- abuse processing

Rspamd DKIM signing is disabled. Citadel signs outbound DKIM with selector `dvbbt`.

The settings rule `outbound_citadel` disables reject, add-header, rewrite-subject and greylist for mail from 127.0.0.1 and ::1. The ARF suppression list can still refuse a recipient.

The configuration intentionally remains conservative and lightweight.

---

## Redis

Redis is used for Bayes statistics and for the ARF suppression list.

There is intentionally no global Redis block. Bayes and `modules.d/motobike_arf.conf` configure Redis for those uses.

Redis memory usage remains intentionally very small.

---

## Unbound

Unbound operates as a local recursive DNS resolver.

Advantages:

- reduced external dependencies
- faster DNS lookups
- improved Rspamd performance
- independent DNS infrastructure

---

## Citadel

Citadel serves as the main communication platform.

Functions include:

- mail storage
- submission on 587 (STARTTLS) and 465 (implicit TLS)
- IMAPS on 993
- DKIM signing with selector `dvbbt` for motobike.fun, g00r00.com and vibedullis.com
- BBS/community features
- messaging
- user management
- long-term content storage

Citadel's smarthost is the localhost Postfix relay on `127.0.0.1:10025`.

Since 2026-10-18, passwords are compared case-sensitively (`c_case_sensitive_passwords=1`).

Citadel uses Berkeley DB instead of large SQL database systems.

---

## WebCit

WebCit provides the browser-based frontend for Citadel.

It runs as two services, `webcit-http` on port 80 and `webcit-https` on port 443.

WebCit serves the MTA-STS policy from `/usr/local/webcit/static/.well-known/mta-sts.txt`.

The interface intentionally remains lightweight and fast.

The platform is actively used as a motorcycle and touring community system.

---

# Backup Architecture

The production server uses daily Hetzner backups.

The backup strategy focuses on:

- a daily recovery point
- reliability
- recovery simplicity

---

# Security Concept

The system uses multiple security layers:

- Hetzner upstream firewall
- UFW host firewall
- WireGuard-only administration
- Fail2ban
- Geo-based filtering
- modern mail authentication standards
- reputation monitoring

---

# Mail Standards

Implemented standards include:

- SPF for `65.109.224.37`
- DKIM selector `dvbbt` (Citadel)
- DMARC `p=reject` / `sp=reject`
- ARC selector `arc2026`
- BIMI
- MTA-STS enforce (`mx` motobike.fun, `max_age` 86400)
- TLS-RPT
- DNSSEC

---

# Philosophy

This project demonstrates that modern communication infrastructure can still be implemented using lightweight classic Unix-oriented architecture without unnecessary complexity.
