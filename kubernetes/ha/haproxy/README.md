# Kubernetes API HA — HAProxy

HAProxy sits in front of the Kubernetes API servers so clients use one stable endpoint instead of connecting to an individual control-plane node.

```text
Client
  │
  ▼
HA endpoint
  │
  ▼
HAProxy
  │
  ├── CP-01:6443
  ├── CP-02:6443
  └── CP-03:6443
```

The real endpoint and addresses are intentionally omitted.

Keepalived provides the highly available virtual IP layer. HAProxy provides the TCP load-balancing layer.

## Why this separation?

- Keepalived handles availability of the virtual IP.
- HAProxy handles API traffic distribution.
- Kubernetes control-plane nodes remain independently addressable internally.
- Clients do not need to know which control-plane node is currently serving the request.
