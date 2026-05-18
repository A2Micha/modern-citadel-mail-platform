# Migrating Citadel Mail Delivery from SMTP to LMTP

## Overview

This document describes the migration of the internal mail delivery path between Postfix and Citadel from SMTP over TCP (`localhost:2025`) to native LMTP delivery over a Unix domain socket.

The migration was performed on a production system running:

- Postfix
- Rspamd
- Redis
- Citadel Groupware
- WebCit
- Fail2Ban
- nftables GeoIP filtering

The goal was to create a cleaner and more modular Unix-style mail architecture while keeping Postfix as the external SMTP edge gateway and Citadel as the backend groupware and message store.

---

# Previous Architecture

Originally, Postfix forwarded mail internally to Citadel using SMTP on TCP port `2025`.

## Mail Flow

```text
Internet
   |
Postfix
   |
Rspamd
   |
SMTP localhost:2025
   |
Citadel
```

## Previous `/etc/postfix/transport`

```text
motobike.fun smtp:[127.0.0.1]:2025
g00r00.com smtp:[127.0.0.1]:2025
basmatibombers.biz smtp:[127.0.0.1]:2025
```

This setup worked reliably and was stable in production.

---

# Why Migrate to LMTP?

Citadel provides native LMTP sockets specifically designed for local mail delivery from external MTAs such as Postfix.

Available sockets:

```text
/usr/local/citadel/lmtp.socket
/usr/local/citadel/lmtp-unfiltered.socket
```

The `lmtp-unfiltered.socket` was selected because spam and malware filtering are already handled externally by Rspamd.

Advantages of LMTP:

- Cleaner local delivery
- Native Unix socket communication
- Avoids unnecessary internal SMTP sessions
- Better architectural separation
- More traditional Unix mail design
- Reduced internal SMTP overhead
- Cleaner Postfix backend integration

---

# New Architecture

```text
Internet
   |
Postfix
   |
Rspamd
   |
LMTP Unix Socket
   |
Citadel
```

Postfix remains responsible for:

- SMTP
- Queue handling
- TLS
- Retry logic
- Internet mail transport

Rspamd remains responsible for:

- Spam filtering
- DKIM
- ARC
- DMARC
- Bayes
- Policy filtering

Citadel remains responsible for:

- Mail storage
- IMAP
- Groupware
- Webmail
- Collaboration

---

# Citadel LMTP Socket Permissions

The Citadel LMTP socket was already present and accessible:

```bash
ls -la /usr/local/citadel/lmtp-unfiltered.socket
```

Output:

```text
srwxrwsrwx 1 root root 0 May 12 21:01 /usr/local/citadel/lmtp-unfiltered.socket
```

This allowed Postfix to connect directly without additional permission changes.

---

# Postfix Configuration Changes

## 1. master.cf

A dedicated LMTP transport was added to `/etc/postfix/master.cf`:

```text
citadel-lmtp unix  -       n       n       -       -       lmtp
  -o lmtp_destination_recipient_limit=1
  -o lmtp_send_xforward_command=yes
```

### Explanation

- `citadel-lmtp`
  - Custom Postfix transport name

- `lmtp_destination_recipient_limit=1`
  - Recommended for LMTP backends

- `lmtp_send_xforward_command=yes`
  - Allows additional SMTP metadata forwarding

---

## 2. transport Maps

`/etc/postfix/transport` was modified:

```text
motobike.fun       citadel-lmtp:unix:/usr/local/citadel/lmtp-unfiltered.socket
g00r00.com         citadel-lmtp:unix:/usr/local/citadel/lmtp-unfiltered.socket
basmatibombers.biz citadel-lmtp:unix:/usr/local/citadel/lmtp-unfiltered.socket
```

---

## 3. main.cf

The existing transport maps remained enabled:

```text
transport_maps = hash:/etc/postfix/transport
```

No virtual mailbox setup was required.

The system continues using domain-based transport routing.

---

# Reloading Postfix

After changes:

```bash
postmap /etc/postfix/transport
postfix check
systemctl reload postfix
```

---

# Testing

Mail delivery was tested using:

- External SMTP delivery
- Multiple domains
- ProtonMail
- Internal test accounts

Mail queue safety was preserved during all testing.

Postfix safely deferred mail during failed LMTP experiments until configuration was corrected.

---

# Successful LMTP Delivery

Successful delivery log example:

```text
postfix/lmtp[263307]: BE6235E111:
to=<michael@motobike.fun>,
relay=motobike.fun[/usr/local/citadel/lmtp-unfiltered.socket],
dsn=2.0.0,
status=sent (250 Message accepted.)
```

Citadel accepted the message directly through LMTP:

```text
citserver:
stat=250 Message accepted.
```

---

# Result

The system now uses native LMTP delivery between Postfix and Citadel.

Final architecture:

```text
Internet
   |
Postfix
   |
Rspamd
   |
LMTP Unix Socket
   |
Citadel
```

This provides a cleaner, more modular, and more traditional Unix mail architecture while preserving the reliability and queue management capabilities of Postfix.

---

# Notes

The migration was performed live on a production system without mail loss.

Postfix queue management made rollback and recovery straightforward during testing.

This demonstrates one of the strengths of classic Unix-style mail infrastructure:
safe incremental changes with reliable queue handling.
