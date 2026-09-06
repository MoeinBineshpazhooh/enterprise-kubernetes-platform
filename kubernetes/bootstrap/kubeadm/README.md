# kubeadm bootstrap

This directory documents the kubeadm-based cluster bootstrap used for the platform.

## Flow

```text
Node preparation
      ↓
kubeadm init
      ↓
Control-plane join
      ↓
Worker join
      ↓
Calico
      ↓
Cluster validation
```

## Expected layout

- `CP-01`, `CP-02`, `CP-03`: control-plane nodes
- `WORKER-*`: worker nodes
- API access is provided through the HA endpoint described in `kubernetes/ha/`.

## Sanitization

All addresses, hostnames and credentials are placeholders. Production kubeconfig files and certificates must never be committed.
