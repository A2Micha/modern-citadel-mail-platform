# Citadel Modernization

## Overview

Citadel is one of the oldest continuously developed collaboration platforms in the Unix world.

Originally rooted in classic Bulletin Board System (BBS) culture, Citadel evolved into a lightweight integrated groupware platform.

This project demonstrates that Citadel can still operate successfully within a modern secure mail infrastructure.

---

# Modernizing a Classic Platform

The platform combines traditional Citadel architecture with modern mail technologies.

Modern components include:

- Postfix SMTP edge handling
- Rspamd filtering
- LMTP local delivery
- Redis integration
- DKIM signing
- ARC signing
- DMARC validation
- WireGuard VPN administration
- nftables filtering
- Fail2Ban protection

This creates a hybrid architecture:

classic groupware philosophy combined with modern Internet mail security.

---

# Why Citadel?

Citadel provides several unique advantages.

## Integrated Architecture

Citadel combines:

- mail
- calendars
- contacts
- messaging
- web access
- groupware

inside a unified platform.

---

## Lightweight Design

The platform remains extremely resource efficient compared to many modern groupware solutions.

Even with modern security layers enabled, the complete infrastructure runs efficiently on small systems.

---

## Unix Philosophy

Citadel follows many classic Unix ideas:

- service-oriented architecture
- efficient local communication
- low overhead
- modular integration
- direct administration

This aligns naturally with native Linux deployments.

---

# LMTP Modernization

One of the most important architectural improvements was migrating local delivery from SMTP to LMTP.

Postfix delivers inbound mail to the unfiltered LMTP socket after the Rspamd milter.

Benefits include:

- direct mailbox validation
- reduced SMTP complexity
- cleaner local delivery
- Unix-style socket communication
- improved queue behavior

This modernizes local mail injection significantly.

---

# Submission and Outbound Mail

Citadel accepts client submission on port 587 (STARTTLS) and port 465 (implicit TLS). IMAPS stays on port 993.

Citadel DKIM-signs outbound mail with selector `dvbbt` for motobike.fun, g00r00.com and vibedullis.com.

The smarthost is a localhost-only Postfix listener at `127.0.0.1:10025` (`postfix/citadel-out`). Postfix runs the Rspamd milter there and delivers the message to the Internet. Rspamd DKIM signing stays disabled, and Rspamd ARC-signs with selector `arc2026`.

Since 2026-10-18, Citadel compares passwords case-sensitively (`c_case_sensitive_passwords=1`).

---

# Certificates

Citadel and WebCit use a Let's Encrypt certificate on ports 443, 465, 587 and 993. Postfix on port 25 uses its own Let's Encrypt certificate.

certbot issues both with the webroot authenticator. The webroot is `/usr/local/webcit/static`. WebCit serves `/.well-known/acme-challenge` on port 80, and `certbot.timer` renews the certificates automatically.

The Citadel and WebCit certificate covers:

- motobike.fun
- mail.motobike.fun
- smtp.motobike.fun
- mta-sts.motobike.fun
- xmpp.motobike.fun
- mail.g00r00.com
- mail.vibedullis.com

`/usr/local/citadel/keys/citadel.cer` and `citadel.key` are symlinks into `/etc/letsencrypt/live/`.

A pre hook makes sure the webroot `.well-known` directory exists. A deploy hook checks those symlinks and restarts `citadel`, `webcit-http` and `webcit-https`. It reloads Postfix when the Postfix certificate renews.

---

# Modern Security Integration

The original Citadel architecture predates many modern mail security standards.

Citadel itself provides submission, DKIM signing and case-sensitive password checks.

The surrounding infrastructure adds:

- SPF validation
- DKIM verification
- DMARC enforcement
- ARC signing
- advanced spam filtering
- reputation systems
- TLS on the public services
- abuse prevention
- recipient verification in Postfix

This preserves the Citadel mailstore while modernizing transport and authentication around it.

---

# Native Linux Deployment

The platform intentionally avoids container abstraction.

Citadel integrates directly into:

- systemd
- Postfix
- LMTP sockets
- Redis
- Linux networking
- nftables
- WireGuard

This preserves transparency and operational simplicity.

---

# Resource Efficiency

The platform demonstrates that modern secure mail infrastructure does not necessarily require large cloud environments or orchestration systems.

The complete stack operates efficiently on:

- VPS systems
- low-resource servers
- ARM hardware
- future RISC-V systems

while maintaining modern filtering and authentication standards.

---

# Preserving the Unix Spirit

The project intentionally preserves:

- transparency
- modularity
- simplicity
- direct system access
- local administration
- infrastructure ownership

The platform follows the belief that administrators should understand their systems instead of depending entirely on abstraction layers.

---

# Conclusion

The Modern Citadel Mail Platform demonstrates that classic Unix-era software can still integrate successfully into modern secure communication infrastructure.

By combining:

- Citadel
- Postfix
- Rspamd
- LMTP
- Redis
- WireGuard
- native Linux services

the platform creates a modernized but transparent self-hosted communication system built on classic Unix principles.
