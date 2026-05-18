# Design Philosophy

## Why This Project Exists

This project is not about building the newest, biggest or most automated mail platform.

It is about proving that classic Unix-style infrastructure can still be modern, secure, efficient and maintainable without depending on heavy cloud ecosystems, large container stacks or complex orchestration platforms.

The goal is to combine traditional Unix design principles with modern mail security standards while keeping the system understandable, transparent and lightweight.

---

# Why Citadel

I like the original Citadel idea very much.

Citadel combines classic groupware functionality with the spirit of old bulletin board systems (BBS).  
Even though the software looks old-fashioned compared to modern web-based collaboration suites, the underlying design is surprisingly efficient and elegant.

Citadel is:

- lightweight
- fast
- simple
- stable
- self-contained

The project avoids unnecessary complexity and follows a more traditional Unix/Linux philosophy.

I also appreciate that Citadel does not depend on large SQL database infrastructures.  
Using Berkeley DB keeps the system compact and resource efficient.

---

# Native Installation Instead of Containers

One of the core ideas behind this project is avoiding unnecessary abstraction layers.

The entire platform is installed natively on Linux without:

- Docker
- Kubernetes
- container orchestration
- large cloud dependencies

This is intentional.

Modern infrastructure often hides complexity behind containers and automation.  
While convenient, this can also disconnect administrators from understanding how the underlying system actually works.

Building the stack natively provides:

- better understanding of the system
- direct access to logs and processes
- easier troubleshooting
- lower resource usage
- reduced dependency chains
- long-term maintainability

The project tries to demonstrate that native Linux services are still completely viable today.

---

# Learning Through Direct System Integration

Another important idea behind this project is education and transparency.

Running services natively forces deeper understanding of:

- SMTP
- LMTP
- DNS
- DKIM
- DMARC
- TLS
- spam filtering
- mail routing
- Unix permissions
- firewalling
- system integration

Instead of relying on prebuilt appliances or cloud products, the administrator learns how the individual components interact internally.

This creates more control and independence from vendors, cloud providers or container ecosystems.

---

# Lightweight Infrastructure

A major design goal is efficiency.

The complete stack runs comfortably on very small systems, including:

- small VPS systems
- Raspberry Pi hardware
- low-power ARM boards
- future RISC-V systems

Even with modern technologies such as:

- Rspamd
- ARC
- DKIM
- DNSSEC
- BIMI
- LMTP
- TLSA
- Redis

the system remains lightweight and resource efficient.

This demonstrates that modern mail infrastructure does not necessarily require large cloud environments or heavy enterprise platforms.

---

# Modular Unix Architecture

The platform follows a modular Unix-style architecture.

Each component has a clearly defined role:

```text
Internet
   |
Postfix
   |
Rspamd
   |
LMTP
   |
Citadel
```

## Postfix

Handles:

- SMTP edge transport
- queue management
- TLS
- retry handling
- external mail transport

## Rspamd

Handles:

- spam filtering
- DKIM signing
- ARC
- DMARC evaluation
- policy enforcement

## Citadel

Handles:

- mailbox storage
- IMAP
- webmail
- groupware functionality
- collaboration

## LMTP

Handles:

- final local message delivery

This separation keeps the system understandable and easier to maintain.

---

# Modern Standards Still Matter

Even though the platform is intentionally lightweight and old-school in spirit, modern mail standards remain extremely important.

The stack therefore integrates:

- SPF
- DKIM
- DMARC
- ARC
- BIMI
- DNSSEC
- TLSA / DANE
- TLSv1.3
- Fail2Ban
- GeoIP filtering
- WireGuard restricted administration

The idea is not nostalgia for old systems.

The goal is combining classic Unix simplicity with modern operational and security standards.

---

# Independence and Ownership

A major motivation behind this project is digital independence.

The platform is designed for people who want:

- ownership of their own infrastructure
- control over their own mail systems
- transparency
- low complexity
- low resource usage
- long-term maintainability

without depending entirely on:

- cloud ecosystems
- hosted groupware platforms
- proprietary SaaS solutions
- container-heavy environments

---

# Final Goal

This project is ultimately an experiment in modern classic infrastructure.

The goal is to show that:

- simple systems can still be powerful
- lightweight systems can still be secure
- native Linux services still work extremely well
- traditional Unix ideas are still relevant
- modern mail standards can coexist with classic architectures

and that understanding the system itself is often more valuable than abstracting everything away.
