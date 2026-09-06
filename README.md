<div align="center">

# ☸️ Enterprise Kubernetes Platform

### Air-Gapped Kubernetes • Network Security • High Availability • Automation

**Ansible → Kubernetes → Calico → Ingress → Workloads**

![Kubernetes](https://img.shields.io/badge/Kubernetes-1.34.x-326CE5?logo=kubernetes&logoColor=white)
![Calico](https://img.shields.io/badge/Network-Calico-0099CC)
![Ansible](https://img.shields.io/badge/Automation-Ansible-EE0000?logo=ansible&logoColor=white)
![Deployment](https://img.shields.io/badge/Deployment-Air--Gapped-success)

A sanitized portfolio representation of a production-oriented Kubernetes platform operated in a restricted environment, covering cluster lifecycle, networking, ingress, storage, security policies, and repeatable upgrades.

</div>

---

## 🧭 Architecture at a Glance

```text
                         🧑‍💻 Automation
                              │
                              ▼
                       ┌─────────────┐
                       │   Ansible   │
                       └──────┬──────┘
                              │
                              ▼
              ┌──────────────────────────────┐
              │       ☸️ Kubernetes          │
              │                              │
              │  ┌────────┐ ┌────────┐       │
              │  │Master 1│ │Master 2│ ...   │
              │  └────────┘ └────────┘       │
              │                              │
              │  ┌────────┐ ┌────────┐       │
              │  │Worker  │ │Worker  │ ...   │
              │  └────────┘ └────────┘       │
              └──────────────┬───────────────┘
                             │
          ┌──────────────────┼──────────────────┐
          ▼                  ▼                  ▼
      🌐 Calico          🚪 Ingress         💾 Storage
          │                  │                  │
          ▼                  ▼                  ▼
   NetworkPolicy       HAProxy-based       Local persistent
   / cluster network     traffic entry         workloads
```

---

## 🧩 Implementation at a Glance

| Capability | Implemented approach |
|---|---|
| ☸️ Kubernetes | Multi-control-plane cluster |
| 🔐 CNI | Calico |
| 🛡️ Network security | Kubernetes NetworkPolicy |
| 🚪 Ingress | HAProxy Ingress Controller |
| 🌐 Load balancing | MetalLB where required |
| 💾 Storage | Local storage / host-backed persistence |
| 🧰 Automation | Ansible |
| 🔄 Lifecycle | kubeadm-based upgrades |
| 📴 Environment | Air-gapped / offline capable |
| 📦 Images | Offline image preparation / private registry |

---

## 🧠 Engineering Decisions

The platform is documented around operational decisions rather than a collection of manifests.

```text
Production requirement
        ↓
Identify failure boundary
        ↓
Choose platform mechanism
        ↓
Automate repeatable work
        ↓
Validate independently
        ↓
Document the operational lesson
```

### Why multiple control-plane nodes?

To avoid making the Kubernetes control plane dependent on a single machine and to provide a foundation for control-plane availability.

### Why Calico?

The platform requires a production-capable CNI with NetworkPolicy support and room for advanced network controls.

### Why isolate network policy?

The cluster follows a restrictive networking model in which application communication is explicitly defined instead of assuming unrestricted east-west traffic.

### Why Ansible?

Cluster preparation and upgrade operations contain many repeatable host-level tasks. Automation reduces configuration drift and makes the operational procedure reproducible.

### Why offline preparation?

The target environment cannot assume internet connectivity. Images, packages, and required artifacts therefore have to be prepared before deployment.

---

## 🛡️ Network Security Model

```text
                    Cluster Network
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
       🚪 Ingress                  🧩 Workloads
             │                         │
             ▼                         ▼
       Allowed paths            NetworkPolicy
                                       │
                              ┌────────┴────────┐
                              ▼                 ▼
                           ALLOW              DENY
```

The design uses Kubernetes NetworkPolicy as a security boundary between workloads.

The principle is simple:

> **Application connectivity should be intentional, observable, and restricted to the required paths.**

---

## 🚪 Ingress & Traffic Flow

```text
External Client
      │
      ▼
┌──────────────┐
│ Load Balancer│
└──────┬───────┘
       │
       ▼
┌──────────────────┐
│ HAProxy Ingress  │
└────────┬─────────┘
         │
         ▼
     Kubernetes
       Service
         │
         ▼
        Pod
```

The implementation separates external traffic entry from application services and workload scheduling.

MetalLB is used where an in-cluster load-balancing mechanism is required.

---

## 💾 Storage Strategy

The platform uses local/host-backed storage for workloads where that model is appropriate.

```text
Pod
 │
 ▼
PVC
 │
 ▼
StorageClass
 │
 ▼
Local / host-backed storage
```

This is deliberately documented as an implementation choice rather than presented as a universal storage recommendation.

Persistent workloads must be evaluated for:

- node dependency;
- recovery requirements;
- backup strategy;
- scheduling constraints;
- data durability.

---

## 🔄 Kubernetes Upgrade Workflow

One of the key operational capabilities is repeatable cluster upgrading.

```text
📦 Offline artifacts
        │
        ▼
🧰 Ansible Controller
        │
        ▼
🔎 Pre-flight validation
        │
        ▼
☸️ Control-plane upgrade
        │
        ▼
🧪 Cluster validation
        │
        ▼
👷 Worker upgrade
        │
        ▼
✅ Final validation
```

The automation keeps the target Kubernetes version explicit and performs the upgrade as a controlled sequence rather than manually changing nodes one by one.

---

## 📴 Air-Gapped Deployment Model

```text
                 📴 Restricted Environment
                          │
          ┌───────────────┴───────────────┐
          ▼                               ▼
   📦 Offline packages              🐳 Container images
          │                               │
          └───────────────┬───────────────┘
                          ▼
                    🧰 Ansible
                          │
                          ▼
                   ☸️ Kubernetes
```

The public repository contains only sanitized examples. Real registry endpoints, credentials, internal addresses, and environment-specific values are intentionally excluded.

---

## 🧯 Operational Failure Boundaries

```text
Host Preparation
      │
      ▼
Control Plane
      │
      ▼
CNI / Networking
      │
      ▼
Ingress
      │
      ▼
Service
      │
      ▼
Pod
      │
      ▼
Storage
```

When a workload fails, troubleshooting starts at the narrowest relevant boundary rather than treating Kubernetes as one large black box.

### Example diagnostic order

1. Node readiness
2. Pod scheduling
3. CNI/network connectivity
4. Service endpoints
5. Ingress routing
6. Storage/PVC state
7. Application logs
8. NetworkPolicy restrictions

---

## 🧰 Practical Problems Solved

### Cluster upgrade automation

Manual upgrades are error-prone and difficult to reproduce. The project uses Ansible to standardize preparation, version selection, upgrade sequencing, and validation.

### Networking restrictions

The cluster requires controlled east-west communication. NetworkPolicy provides an explicit authorization layer for workload traffic.

### Ingress topology

External traffic needs a predictable entry path. HAProxy Ingress and load-balancing components provide that boundary without coupling applications directly to external infrastructure.

### Offline deployment

Internet-dependent installation is unsuitable for the target environment. Required packages and images are staged before deployment.

### Storage scheduling constraints

Local storage introduces node affinity and recovery considerations. Persistent workloads therefore need to be evaluated together with scheduling and failure behavior.

---

## 🔎 Verification Strategy

```text
❶ Nodes are Ready
        ↓
❷ CNI is healthy
        ↓
❸ Core services are healthy
        ↓
❹ Ingress controller is Ready
        ↓
❺ Services have endpoints
        ↓
❻ Pods can communicate as designed
        ↓
❼ PVCs bind successfully
        ↓
❽ Application traffic succeeds
```

Typical checks:

```bash
kubectl get nodes
kubectl get pods -A
kubectl get svc -A
kubectl get ingress -A
kubectl get pvc -A
kubectl get networkpolicy -A
```

---

## 🔐 Security Rules

```text
                 Public Repository
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
           ✅ Keep                 ❌ Never
              │                     │
       fake addresses          real credentials
       placeholders             private keys
       example values           internal domains
       sanitized manifests      production secrets
```

Security principles represented by the project:

- least-privilege workload communication;
- restricted network paths;
- external secrets kept outside Git;
- offline artifact preparation;
- explicit cluster access boundaries.

---

## 📁 Repository Roadmap

```text
enterprise-kubernetes-platform/
│
├── README.md
├── ansible/
│   ├── inventory/
│   ├── roles/
│   └── playbooks/
│
├── kubernetes/
│   ├── cluster/
│   ├── networking/
│   ├── ingress/
│   ├── storage/
│   └── security/
│
├── offline/
│   ├── packages/
│   └── images/
│
├── scripts/
└── docs/
    ├── architecture.md
    ├── upgrades.md
    ├── networking.md
    ├── storage.md
    └── troubleshooting.md
```

The repository will be populated only with configurations and procedures that reflect the actual implementation.

---

## 🎯 Interview Focus

This project is designed to support practical DevOps/Platform Engineering discussions:

- How do you design a multi-control-plane Kubernetes cluster?
- What does Calico provide beyond basic pod networking?
- How do NetworkPolicies change the cluster security model?
- How do you troubleshoot a Pod stuck in `Pending`?
- How do you troubleshoot an ingress path that returns no application response?
- What are the risks of local persistent storage?
- How do you perform a kubeadm upgrade safely?
- How do you upgrade an air-gapped Kubernetes cluster?
- How do you prepare images and packages without internet access?
- How do you validate a cluster after an upgrade?

The answers should come from the implementation and operational lessons rather than generic Kubernetes theory.

---

## 🔒 Portfolio Safety

This repository is intentionally sanitized. Real infrastructure names, addresses, registry endpoints, credentials, tokens, certificates, application names, and employer/project identifiers are not included.

No MVB, employer, internal domain, or production topic names are used.

---

<div align="center">

### 🧰 Ansible → ☸️ Kubernetes → 🛡️ Calico → 🚪 Ingress → 📦 Workloads

**Automated. Restricted. Repeatable. Operationally explainable.**

</div>
