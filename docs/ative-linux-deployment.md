# Native Linux Deployment

## Overview

This platform intentionally avoids container-based deployment models.

Instead of Docker, Kubernetes or prebuilt mail appliances, the entire stack is deployed directly on native Linux using traditional Unix service separation.

The goal is not minimalism for its own sake.

The goal is:

- transparency
- maintainability
- efficiency
- operational control
- long-term reliability

---

# Philosophy

Modern infrastructure increasingly depends on abstraction layers.

This platform follows the opposite approach.

The architecture is intentionally designed around:

- native Linux services
- local socket communication
- Unix process separation
- transparent logging
- direct service management
- low dependency count

Every component can be individually:

- inspected
- restarted
- debugged
- monitored
- upgraded

without orchestration frameworks or container runtimes.

---

# Why No Docker?

Container platforms solve many deployment problems.

However, they also introduce:

- abstraction layers
- additional networking complexity
- hidden dependencies
- storage indirection
- operational overhead
- more difficult troubleshooting

For this platform, native Linux deployment provides significant advantages.

## Advantages

- lower RAM usage
- lower CPU overhead
- reduced storage consumption
- direct filesystem visibility
- native systemd integration
- simpler backups
- transparent logs
- easier debugging
- long-term maintainability

---

# Unix Service Separation

The platform follows classic Unix principles.

Each service performs a dedicated task.

| Service | Purpose |
|---|---|
| Postfix | SMTP transport and queue management |
| Rspamd | Filtering and policy engine |
| Redis | Statistical backend |
| Citadel | Mailstore and collaboration |
| WireGuard | Administrative VPN |
| nftables | Packet filtering |
| Fail2Ban | Abuse prevention |

This modularity improves:

- observability
- reliability
- fault isolation
- operational clarity

---

# Local Socket Communication

Internal services communicate using local sockets whenever possible.

Examples:

- Postfix → Rspamd Milter
- Postfix → LMTP socket
- Citadel local delivery

Benefits include:

- lower overhead
- reduced attack surface
- improved performance
- simplified local trust boundaries

---

# Resource Efficiency

The platform is intentionally lightweight.

Even with:

- Rspamd
- Redis
- ARC signing
- DKIM
- DMARC
- Fail2Ban
- WireGuard
- LMTP delivery

resource usage remains low.

This makes the platform suitable for:

- VPS systems
- small dedicated servers
- Raspberry Pi deployments
- ARM systems
- future RISC-V systems

---

# Learning and Transparency

Native Linux deployment provides educational value.

Administrators interact directly with:

- queues
- sockets
- logs
- services
- firewall layers
- mail transport internals

This improves understanding of:

- SMTP architecture
- Unix service models
- Linux networking
- mail security
- system integration

The platform encourages understanding rather than abstraction.

---

# Long-Term Maintainability

The stack minimizes external dependencies.

No dependency exists on:

- container registries
- orchestration frameworks
- cloud APIs
- external deployment systems

This reduces operational risk and improves long-term sustainability.

---

# Conclusion

The Modern Citadel Mail Platform demonstrates that modern secure communication infrastructure can still be deployed successfully using classic Unix principles.

Native Linux deployment provides:

- transparency
- efficiency
- stability
- independence
- operational simplicity

while maintaining compatibility with modern mail security standards and infrastructure requirements.
