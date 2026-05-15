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
          +-----------------+-----------------+
          |                                   |
       Postfix                            WireGuard
          |                                   |
          |                              SSH Access
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

```

---

# Component Description

## Hetzner Firewall

The Hetzner upstream firewall filters unwanted traffic before it reaches the server infrastructure.

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

Postfix acts as the SMTP frontend for incoming and outgoing mail transport.

Responsibilities include:

- SMTP handling
- mail routing
- TLS handling
- policy enforcement
- integration with Rspamd

---

## Rspamd

Rspamd handles:

- spam filtering
- SPF validation
- DKIM validation
- DMARC validation
- ARC processing
- greylisting
- reputation analysis
- abuse processing

The configuration intentionally remains conservative and lightweight.

---

## Redis

Redis is used as a lightweight backend for Rspamd features including:

- Bayes statistics
- reputation data
- temporary filtering data

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
- BBS/community features
- messaging
- user management
- long-term content storage

Citadel uses Berkeley DB instead of large SQL database systems.

---

## WebCit

WebCit provides the browser-based frontend for Citadel.

The interface intentionally remains lightweight and fast.

The platform is actively used as a motorcycle and touring community system.

---

# Backup Architecture

Backups are performed using Borg Backup within the Hetzner infrastructure.

The backup strategy focuses on:

- incremental backups
- low storage usage
- fast recovery
- long-term reliability

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

- SPF
- DKIM
- DMARC
- ARC
- BIMI
- MTA-STS
- TLS-RPT
- DNSSEC
- DANE/TLSA (prepared)

---

# Philosophy

This project demonstrates that modern communication infrastructure can still be implemented using lightweight classic Unix-oriented architecture without unnecessary complexity.
