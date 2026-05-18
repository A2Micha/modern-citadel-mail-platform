```mermaid
flowchart TD
    A[Internet] --> B[Hetzner Firewall]
    B --> C[UFW]
    C --> D[nftables GeoIP]
    D --> E[Fail2Ban]
    E --> F[Postfix]
    F --> G[Rspamd]
    G --> H[Citadel]
```
