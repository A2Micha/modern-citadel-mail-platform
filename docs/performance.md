# Performance

## Overview

The Modern Citadel Mail Platform is designed to run efficiently on lightweight infrastructure.

The current production environment runs on:

- Hetzner CX22
- 2 vCPU
- 4 GB RAM
- 40 GB SSD

Despite running multiple native services simultaneously, the system maintains very low resource consumption.

---

# Production htop Snapshot

![Production htop](../screenshots/htop-production.png)

---

# Observed Runtime Characteristics

The platform typically shows:

- very low CPU usage
- low I/O activity
- stable RAM utilization
- minimal background load
- long-term uptime stability

The system currently runs:

- Citadel
- Postfix
- Rspamd
- Redis
- Fail2Ban
- WireGuard
- WebCit

without containerization or orchestration frameworks.

---

# Native Linux Service Design

The platform intentionally avoids:

- Docker
- Kubernetes
- overlay filesystems
- container abstraction layers

This reduces:

- RAM overhead
- storage complexity
- CPU overhead
- operational abstraction

while improving:

- transparency
- maintainability
- troubleshooting
- hardware efficiency

---

# Lightweight Infrastructure Philosophy

A core design goal is proving that modern and secure mail infrastructure does not require oversized cloud environments.

The platform is intentionally designed to run efficiently on:

- small virtual machines
- low-power x86 systems
- Raspberry Pi
- lightweight Unix/Linux environments
- future RISC-V platforms

while still supporting:

- modern TLS
- DKIM
- ARC
- DMARC
- Rspamd filtering
- secure VPN administration
- modern SMTP standards
