# WKD (Web Key Directory) Integration

## Overview

The platform implements OpenPGP WKD (Web Key Directory) support for automatic public key discovery.

WKD allows compatible mail clients and OpenPGP implementations to retrieve public encryption keys directly from the domain infrastructure using HTTPS.

This enables:

- automatic OpenPGP key discovery
- simplified encrypted mail communication
- trusted HTTPS-based key retrieval
- reduced dependency on public keyservers

The implementation follows modern OpenPGP and WKD standards using native Linux tooling.

---

# Why WKD?

Traditional OpenPGP key distribution often depends on:

- public keyservers
- manual key exchange
- external infrastructure
- unreliable synchronization

WKD provides domain-controlled public key discovery directly through HTTPS.

Advantages include:

- domain ownership verification
- simplified client integration
- reduced spoofing risk
- cleaner key distribution
- better interoperability

---

# Native Linux Integration

The WKD implementation was deployed directly on the native Linux infrastructure without external services or container abstraction.

The integration uses:

- GnuPG
- WebCit static file handling
- HTTPS transport
- native filesystem structure

This aligns with the overall platform philosophy:

- transparency
- direct infrastructure ownership
- native Linux deployment
- minimal abstraction

---

# WKD Directory Structure

The WKD directory structure was created directly inside the WebCit static content directory.

```text
/usr/local/webcit/.well-known/openpgpkey/
```

Structure:

```text
.well-known/
└── openpgpkey/
    ├── policy
    └── hu/
        └── <WKD_HASH>
```

The `policy` file enables WKD discovery support.

---

# WKD Policy File

The platform publishes a WKD policy file:

```text
https://motobike.fun/.well-known/openpgpkey/policy
```

Example:

```text
# WKD policy file
```

The existence of this file confirms WKD support for compatible clients.

---

# GnuPG Key Generation

The OpenPGP key was generated using modern elliptic curve cryptography.

Generated key type:

```text
ed25519
cv25519
```

Properties:

- Ed25519 signing key
- Curve25519 encryption subkey
- modern ECC cryptography
- reduced key size
- strong security properties

Generated identity:

```text
Michael <michael@motobike.fun>
```

Observed key fingerprint:

```text
80626BF28DD00F966486F4A4D4D99663744BE9A3
```

---

# WKD Hash Generation

WKD uses a deterministic hash derived from the mail address.

Hash generation:

```bash
/usr/lib/gnupg/gpg-wks-client --print-wkd-hash michael@motobike.fun
```

Generated WKD hash:

```text
n6h6dt1ftdd9w3y3sujj5oxwejt8uqob
```

This hash becomes the public key filename.

---

# Public Key Publication

The exported OpenPGP public key is published at:

```text
https://motobike.fun/.well-known/openpgpkey/hu/n6h6dt1ftdd9w3y3sujj5oxwejt8uqob
```

The key was exported using:

```bash
gpg --export michael@motobike.fun > pubkey.gpg
```

and published directly through WebCit static HTTPS delivery.

---

# WKD Validation

The deployment was successfully validated using:

```bash
curl
```

and:

```bash
gpg --locate-keys
```

Example:

```bash
gpg --auto-key-locate clear,wkd --locate-keys michael@motobike.fun
```

Result:

```text
pub   ed25519
uid   Michael <michael@motobike.fun>
```

This confirms:

- successful WKD discovery
- correct HTTPS delivery
- valid OpenPGP key retrieval
- standards-compliant implementation

---

# HTTPS Transport

WKD retrieval occurs entirely over HTTPS.

Benefits:

- encrypted transport
- integrity protection
- certificate validation
- domain ownership verification

The platform already implements:

- TLS
- MTA-STS
- TLS-RPT
- modern cipher suites

which complements secure WKD operation.

---

# Integration Philosophy

WKD integrates naturally into the platform philosophy.

The implementation demonstrates:

- modern cryptographic interoperability
- transparent Unix infrastructure
- standards-based communication
- self-hosted trust management
- native Linux deployment

without dependency on:

- external OpenPGP keyservers
- cloud APIs
- third-party synchronization infrastructure

---

# Security Considerations

The WKD implementation benefits from:

- HTTPS-only transport
- local infrastructure control
- WireGuard administrative isolation
- native Linux filesystem permissions
- minimal exposed surface

This reduces operational complexity while maintaining standards compliance.

---

# Future Goals

Possible future improvements include:

- automatic WKD key rotation
- multiple domain support
- automated key publishing
- OpenPGP mail workflow integration
- additional mail client interoperability testing

---

# Conclusion

The WKD deployment demonstrates how modern OpenPGP key discovery can be integrated directly into a native Linux mail platform.

Combined with:

- Postfix
- Citadel
- Rspamd
- DKIM
- DMARC
- ARC
- MTA-STS
- TLS-RPT

the platform now supports modern encrypted mail trust infrastructure using transparent Unix-based architecture.
