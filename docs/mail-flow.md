# Mail Flow

## Overview

The Modern Citadel Mail Platform uses a modular mail flow architecture designed around clear separation of responsibilities.

Instead of combining all mail handling functions into a single monolithic application, the platform separates:

- SMTP transport
- spam filtering
- queue management
- local delivery
- mailbox storage

into specialized Unix services.

This improves:

- maintainability
- transparency
- troubleshooting
- modularity
- long-term reliability

---

# Inbound Mail Flow

Incoming mail follows this path:

```text
Internet
   |
Postfix SMTP Edge
   |
Rspamd Filtering
   |
LMTP Delivery
   |
Citadel Mailstore
```

---

## 1. Postfix SMTP Edge

Postfix acts as the public SMTP entry point.

Responsibilities:

- inbound SMTP handling
- TLS encryption
- queue management
- retry handling
- relay restrictions
- protocol validation

Postfix intentionally operates as the external mail transport layer while Citadel focuses on mailbox and groupware functionality.

---

## 2. Rspamd Filtering

Rspamd is integrated through Postfix milters.

Responsibilities:

- spam detection
- DKIM signing
- ARC signing
- DMARC evaluation
- reputation analysis
- DNSBL checks
- header analysis

Rspamd processes mail before final local delivery.

---

## 3. LMTP Local Delivery

After filtering, Postfix delivers mail to Citadel using LMTP.

```text
Postfix -> LMTP -> Citadel
```

LMTP replaces the older SMTP-based local handoff previously used between Postfix and Citadel.

Advantages of LMTP:

- direct mailbox delivery
- per-recipient delivery status
- cleaner local architecture
- reduced SMTP complexity internally
- improved separation between SMTP transport and mailbox handling

Citadel provides native LMTP sockets for local message injection.

---

## 4. Citadel Mailstore

Citadel handles:

- mailbox storage
- IMAP access
- message indexing
- webmail
- groupware functionality
- collaboration features

Citadel acts as the final message destination within the platform.

---

# Outbound Mail Flow

Outgoing mail follows this path:

```text
Mail Client
   |
Citadel Submission
   |
Postfix
   |
Rspamd DKIM/ARC
   |
Internet
```

---

## Outbound Responsibilities

### Citadel

Handles:

- authenticated submission
- user access
- message composition
- mailbox management

### Postfix

Handles:

- outbound SMTP transport
- remote TLS negotiation
- queue retries
- external routing

### Rspamd

Handles:

- DKIM signing
- ARC signing
- outbound filtering policies

---

# Security Integration

The mail flow integrates multiple security layers:

- TLSv1.3
- SPF
- DKIM
- DMARC
- ARC
- DNSSEC
- DANE/TLSA
- Fail2Ban
- GeoIP filtering

This allows the platform to remain lightweight while still supporting modern mail security standards.

---

# Design Philosophy

The mail flow intentionally follows classic Unix principles:

- small specialized services
- clear separation of responsibilities
- transparent infrastructure
- minimal abstraction layers
- native Linux integration

The goal is not maximum complexity.

The goal is reliability, maintainability and operational transparency.
