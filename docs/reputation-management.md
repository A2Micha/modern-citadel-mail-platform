# Reputation Management

## Overview

Modern mail infrastructure is no longer based only on spam filtering.

Reliable mail delivery today depends heavily on reputation management, complaint processing and adaptive filtering.

This platform integrates:

- Rspamd
- Redis
- Microsoft SNDS
- Microsoft JMRP
- ARF processing
- custom Lua modules

to build a modern reputation-aware filtering architecture.

The goal is not only filtering spam.

The goal is maintaining long-term sender reputation and improving mail deliverability automatically.

---

# Why Reputation Matters

Large providers such as:

- Microsoft
- Google
- Yahoo
- Proton
- Apple

heavily rely on sender reputation.

Important factors include:

- complaint rates
- spamtrap hits
- DKIM quality
- SPF alignment
- DMARC policies
- historical sender behavior
- URL reputation
- attachment reputation
- mail consistency

Modern mail systems must continuously monitor and react to reputation signals.

---

# Microsoft SNDS

Microsoft SNDS is set up for the server IP.

Microsoft Smart Network Data Services (SNDS) provides visibility into how Microsoft views sending IP addresses.

SNDS exposes information such as:

- IP reputation
- complaint statistics
- spamtrap activity
- filtering status
- mail volume
- sender quality

This allows continuous monitoring of Microsoft-facing deliverability.

SNDS acts as an external reputation telemetry system for Outlook, Hotmail and Microsoft 365 infrastructure.

---

# Microsoft JMRP

Microsoft JMRP is set up for the server IP.

Microsoft Junk Mail Reporting Program (JMRP) provides complaint feedback loops.

When users click:

"Report as Spam"

Microsoft generates feedback reports using:

- ARF (Abuse Reporting Format)

These reports contain valuable reputation information including:

- original message headers
- recipient complaints
- sender metadata
- DKIM context
- SPF information

This creates a direct feedback channel from recipient behavior back into the filtering system.

---

# Abuse Reporting Format (ARF)

ARF is used to process spam complaints automatically.

The platform uses ARF reports to improve:

- reputation awareness
- filtering behavior
- future message handling
- abuse response workflows

Since 2026-10-18, incoming ARF reports (`multipart/report`, `message/feedback-report`) are handled by `motobike_arf.lua`.

The module reads `Original-Rcpt-To` and stores a Redis key `suppression:<address>` for 180 days. Redis for this module is configured in `modules.d/motobike_arf.conf`. The script is loaded from `rspamd.local.lua`.

Later outbound mail via `postfix/citadel-out` to that address is rejected with `554 5.7.1 Recipient suppressed (ARF complaint)`.

ARF processing provides visibility into:

- user complaints
- problematic senders
- compromised accounts
- abusive traffic patterns

---

# Rspamd Reputation Integration

Rspamd forms the central reputation and filtering engine.

The platform integrates:

- Redis for Bayes and ARF suppression
- Bayesian learning
- fuzzy hashing
- reputation systems
- DNSBL analysis
- SURBL analysis
- neural learning
- greylisting
- Lua extensions

This allows dynamic and adaptive filtering behavior.

---

# Redis Backend

Redis is used for Bayes statistics and for the ARF suppression list.

There is intentionally no global Redis block. The ARF module configures its Redis server in `modules.d/motobike_arf.conf`.

Redis enables fast lookups for those two uses.

---

# Custom Lua Integration

The platform includes custom Lua-based Rspamd modules.

## motobike_arf

```ucl
motobike_arf {
    enabled = true;
}
```

`motobike_arf.lua` turns an ARF complaint into a 180-day recipient suppression. Outbound delivery to a suppressed address stops on the Citadel relay with `554 5.7.1 Recipient suppressed (ARF complaint)`.

Complaint handling sits in the same filtering pipeline as the outbound relay.

---

# Adaptive Filtering Philosophy

Filtering should never be static.

Modern spam filtering must continuously adapt to:

- complaint behavior
- sender reputation
- changing spam campaigns
- phishing techniques
- provider reputation scoring
- user feedback

The platform therefore focuses on:

- learning systems
- reputation telemetry
- complaint integration
- adaptive filtering behavior

instead of relying solely on static rules.

---

# Queue and Reputation

Queue-based mail architecture also supports reputation management.

Temporary delivery problems can occur due to:

- greylisting
- reputation throttling
- remote provider delays
- adaptive filtering decisions

The Postfix queue absorbs these temporary conditions safely.

This improves long-term delivery stability.

---

# Deliverability Goals

The platform focuses on maintaining strong deliverability across major providers.

This includes:

- SPF alignment for `65.109.224.37`
- Citadel DKIM signing with selector `dvbbt`
- DMARC `p=reject` and `sp=reject`
- Rspamd ARC signing with selector `arc2026`
- TLS consistency
- stable reverse DNS
- low complaint rates
- ARF recipient suppression

Verified delivery on 2026-10-18:

- Gmail reports `dkim=pass` (`dvbbt`), `spf=pass`, `dmarc=pass` (`p=reject`), `arc=pass` and TLS 1.3
- Microsoft/Outlook reports SPF, DKIM and DMARC pass

Deliverability is treated as an operational process rather than a one-time configuration task.

---

# Administrative Security

Reputation infrastructure is protected through:

- WireGuard administrative access
- localhost-only Rspamd controller
- segmented firewall layers
- Fail2Ban abuse protection
- restricted administrative exposure

This reduces the attack surface significantly.

---

# Long-Term Vision

The platform is designed to evolve toward increasingly adaptive mail reputation handling.

Future goals include:

- reputation scoring
- provider-specific reputation analysis
- self-learning filtering behavior
- automated abuse workflows
- advanced ARF correlation
- intelligent sender trust handling

---

# Conclusion

Modern mail infrastructure depends heavily on reputation management.

By integrating:

- Rspamd
- Redis
- SNDS
- JMRP
- ARF processing
- Lua automation

the platform moves beyond traditional spam filtering toward adaptive reputation-aware mail handling.

This approach combines:

- classic Unix architecture
- modern filtering technology
- provider reputation telemetry
- self-hosted infrastructure
- transparent operational control

into a modern native Linux mail platform.
