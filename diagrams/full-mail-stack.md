```mermaid
flowchart TD

subgraph Security Layer
A[Hetzner Firewall]
B[UFW]
C[nftables / GeoIP]
D[Fail2Ban]
end

subgraph Mail Transport
E[Postfix SMTP :25]
F[Postfix Queue]
G[Rspamd Milter]
H[Unfiltered LMTP]
O[postfix/citadel-out]
end

subgraph Groupware Layer
I[Citadel Mailstore]
J[WebCit 80/443]
K[IMAP 993 / Submission 587/465]
end

A --> B --> C --> D --> E
E --> F --> G --> H --> I

K --> I
I -->|DKIM dvbbt| O
O -->|ARC arc2026| N[Internet]

I --> J
```

# Additional Architecture Diagrams

## Internet Entry Point

```mermaid
flowchart TD

X[Internet]
--> A[Hetzner Firewall]

A --> B[UFW]
B --> C[nftables / GeoIP]
C --> D[Fail2Ban]
```

---

## Administrative Access

```mermaid
flowchart LR

W[WireGuard VPN]
--> S[Administrative Access]

S --> P[SSH]
S --> R[Rspamd WebUI]
S --> C[Citadel Administration]
```

---

## Client Access Layer

```mermaid
flowchart TD

A[Citadel Mailstore]

A --> B[WebCit HTTP/HTTPS]
A --> C[IMAPS 993]
A --> D[Submission 587/465]
```
