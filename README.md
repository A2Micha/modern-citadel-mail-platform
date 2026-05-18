# modern-citadel-mail-platform
Modern self-hosted Citadel communication platform with Postfix, Rspamd, ARC, BIMI, DNSSEC, modern mail security standards and classic Unix architecture.

# Modern Citadel Mail Platform

## Overview

This project documents a modern self-hosted communication platform built around Citadel, Postfix and Rspamd.

The goal of this project is not to build the next cloud-scale groupware platform, but to demonstrate how modern mail security standards and reliable communication services can still be implemented using a classic Unix-oriented architecture.

The system is intentionally designed to remain:

- lightweight
- understandable
- resource efficient
- transparent
- stable over long periods of time

Instead of relying on large container stacks, multiple databases, complex orchestration systems or heavyweight web frameworks, this platform uses small specialized Unix services working together in a predictable way.

---

# Philosophy

Modern communication infrastructure does not necessarily require excessive complexity.

This project intentionally avoids:

- Kubernetes
- heavy container orchestration
- oversized SQL infrastructures
- unnecessary JavaScript frameworks
- resource-heavy groupware stacks

Instead, the platform focuses on:

- classic Unix principles
- direct architecture
- low resource usage
- long-term maintainability
- transparent system behavior
- independent self-hosted communication

Citadel itself may appear oldschool at first glance, but this is actually one of its greatest strengths.

The platform is:

- fast
- simple
- stable
- easy to maintain
- extremely lightweight

Citadel uses Berkeley DB instead of large SQL infrastructures and operates with very low overhead.

This results in a communication platform that remains responsive and efficient even on relatively small systems.

---

# Main Components

| Component | Purpose |
|---|---|
| Citadel | Communication platform / Mail / BBS |
| WebCit | Web interface |
| Postfix | SMTP frontend |
| Rspamd | Spam filtering and mail authentication |
| Redis | Rspamd backend |
| Unbound | Local recursive DNS resolver |
| Fail2ban | Attack detection and protection |
| WireGuard | Administrative VPN access |
| Borg Backup | Automated backup system |
| UFW | Local firewall |
| Hetzner Firewall | Upstream firewall protection |

---

# Mail Security Standards

The platform implements modern mail authentication and transport security standards including:

- SPF
- DKIM
- DMARC
- ARC
- BIMI
- MTA-STS
- TLS-RPT
- DNSSEC
- DANE/TLSA (prepared)

Additional reputation and monitoring services:

- Google Postmaster Tools
- Microsoft SNDS
- DMARC reporting and analysis
- ARC validation
- TLS reporting

DMARC reports are automatically processed using onesecure.net.

---

# Security Architecture

## Network Protection

The system uses multiple layers of protection.

### Hetzner Firewall

An upstream Hetzner firewall filters unwanted traffic before it reaches the server.

### UFW

An additional local UFW firewall provides host-level filtering.

### WireGuard-only SSH

SSH access is NOT publicly exposed.

Administrative access is only possible through WireGuard VPN.

This significantly reduces the external attack surface.

### Geo Blocking

Geo-based filtering is applied to SSL-based mail services such as:

- IMAPS
- POP3S
- Submission

Only SMTP port 25 is globally reachable for mail transport.

### Fail2ban

Fail2ban protects:

- SMTP
- IMAP
- POP3
- Web services

against:

- brute force attacks
- login abuse
- suspicious authentication behavior

---

# Rspamd Configuration Philosophy

Rspamd is intentionally configured in a conservative and resource-efficient way.

The platform uses:

- Bayes filtering
- DNSBL/RBL
- SPF/DKIM/DMARC validation
- Greylisting
- Rate limiting
- ARC processing
- ARF/abuse handling

The setup intentionally avoids overly aggressive neural learning configurations.

This keeps:

- Redis usage extremely small
- system behavior predictable
- false positives low
- resource consumption minimal

Redis typically uses only around 1 MB of actual dataset memory.

---

# ARF and Abuse Processing

The platform already processes:

- ARF reports
- abuse feedback loops
- complaint reports
- automated reputation feedback

Additional custom Lua logic is used within the Rspamd environment to automate abuse handling and reputation-related workflows.

---

# DNS Infrastructure

## Local Recursive Resolver

Unbound operates locally on the server.

Advantages:

- faster DNS lookups
- independent resolver infrastructure
- improved Rspamd performance
- reduced external dependencies
- complete DNS visibility

---

# Community Platform

This system is not used only as a mail server.

Citadel and WebCit are also used as a motorcycle and touring community platform.

The system hosts:

- motorcycle information
- tour documentation
- technical discussions
- community content
- long-term archived knowledge

This creates a classic BBS-style communication environment combined with modern mail infrastructure.

---

# Backup Strategy

Backups are handled automatically using Borg Backup within the Hetzner infrastructure.

The backup strategy focuses on:

- reliable incremental backups
- low storage overhead
- fast recovery
- long-term data integrity

Backed up data includes:

- Citadel data
- Berkeley DB content
- mail data
- configuration files
- system configuration

---

# Monitoring

Monitoring is intentionally kept simple and lightweight.

Basic infrastructure monitoring is performed using the Hetzner dashboard.

Additionally, reputation and mail flow monitoring includes:

- Google Postmaster Tools
- Microsoft SNDS
- DMARC analysis
- TLS reporting
- ARC consistency validation

---

# Resource Usage

One of the main goals of this platform is maintaining low resource consumption while still supporting modern mail standards.

Typical production operation:

- extremely low CPU load
- approximately 1 GB total RAM usage
- very small Redis footprint
- long uptime
- stable long-term behavior

The system intentionally demonstrates that modern mail infrastructure does not require massive hardware resources.

---

# Why This Project Exists

This project exists because many modern communication platforms have become unnecessarily complex.

The goal is to demonstrate an alternative approach:

- classic Unix architecture
- independent self-hosting
- modern mail standards
- low complexity
- transparent infrastructure
- long-term maintainability

The platform is designed for real-world productive use and not as a short-lived laboratory setup.

---

# Current Status

The platform is already running productively and continuously.

Implemented features include:

- modern mail authentication
- ARC signing and validation
- BIMI
- DNSSEC
- automated DMARC analysis
- abuse processing
- multi-layer firewalling
- VPN-only administrative access
- automated backups
- lightweight monitoring

---

# Future Documentation

Additional documentation planned:

- Postfix configuration examples
- Rspamd configuration
- DNS examples
- WireGuard setup
- Fail2ban configuration
- UFW rules
- ARC signing setup
- BIMI implementation
- DANE/TLSA examples
- abuse automation
- backup and restore procedures
- architecture diagrams

---

# License

MIT License

---

# Final Thoughts

This project is not about nostalgia.

It is about proving that modern communication standards and reliable self-hosted infrastructure can still be built using simple, understandable and efficient Unix-based architectu


## Documentation

- [Platform Architecture](docs/platform-architecture.md)
- [Postfix LMTP Migration](docs/postfix-lmtp-migration.md)
