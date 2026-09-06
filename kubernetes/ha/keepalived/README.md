# Kubernetes API HA — Keepalived

Keepalived is used with HAProxy to provide a highly available virtual IP for Kubernetes API access.

```text
              Virtual IP
                  │
          ┌───────┴───────┐
          ▼               ▼
      HAProxy-01       HAProxy-02
          │               │
          └───────┬───────┘
                  ▼
          Kubernetes API
```

Only the generic topology is published. Real virtual IPs, interface names and hostnames are excluded.

The HAProxy pair and the three Kubernetes control-plane nodes are separate availability layers.
