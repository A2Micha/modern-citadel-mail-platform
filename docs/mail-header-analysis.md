# Mail Header Analysis

This document demonstrates a real production mail flow through the Modern Citadel Mail Platform.

The following example shows interoperability between:

- Microsoft Outlook / Exchange Online
- Postfix SMTP Edge
- Rspamd filtering
- ARC handling
- DKIM validation
- TLS secured transport
- Citadel local delivery

---

# Transport Flow

The message passed through:

```text
Outlook / Exchange Online
        ↓
Microsoft outbound protection
        ↓
Postfix SMTP Edge
        ↓
Rspamd filtering
        ↓
LMTP local delivery
        ↓
Citadel Mailstore
```

---

# SMTP TLS Transport

The production environment uses encrypted SMTP transport with modern cipher suites.

Example:

```text
TLS1_2
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
```

This confirms:

- modern TLS negotiation
- encrypted SMTP sessions
- secure relay interoperability
- compatibility with Microsoft Exchange Online

---

# ARC Handling

The platform supports ARC (Authenticated Received Chain).

Observed headers:

```text
ARC-Seal
ARC-Message-Signature
ARC-Authentication-Results
```

This allows preservation of authentication trust across forwarding scenarios.

ARC support improves interoperability with:

- Microsoft 365
- Outlook.com
- forwarding environments
- mailing lists
- complex SMTP routing paths

---

# DKIM Validation

Observed DKIM signature:

```text
DKIM-Signature: d=OUTLOOK.DE
```

This confirms successful DKIM handling and trusted message authentication.

The platform validates and processes:

- DKIM
- SPF
- DMARC
- ARC

using Rspamd.

---

# SMTP Edge Processing

The Postfix SMTP edge performs:

- queue management
- TLS negotiation
- SMTP policy enforcement
- local routing
- Rspamd integration
- LMTP forwarding

Observed:

```text
by motobike.fun (Postfix)
```

This demonstrates clean separation between:

- SMTP transport
- filtering
- local delivery
- mailbox storage

---

# Rspamd Analysis

Observed result:

```text
X-Spam-Status: No, score=-1.30
```

This indicates:

- successful spam evaluation
- low spam probability
- conservative filtering behavior
- stable reputation handling

The filtering pipeline is intentionally designed to minimize false positives.

---

# Microsoft Interoperability

The observed headers demonstrate successful interoperability with Microsoft infrastructure:

- Outlook.com
- Exchange Online Protection
- Microsoft ARC handling
- Microsoft DKIM validation
- Microsoft TLS transport

This confirms compatibility with large enterprise mail ecosystems.

---

# Architecture Benefits

The current architecture provides:

- transparent SMTP handling
- queue separation
- native Linux observability
- modern mail authentication
- reputation-aware filtering
- standards-compliant interoperability

without container orchestration or external mail gateways.

---

# Conclusion

The production mail flow demonstrates that modern Unix-based mail infrastructure can still provide:

- secure transport
- trusted authentication
- reputation-aware filtering
- enterprise interoperability
- reliable local delivery

using transparent native Linux services.
