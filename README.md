<div align="center">

# ☸️ Enterprise Kubernetes Platform

### Kubeadm • Calico • HAProxy • Keepalived • Longhorn • Ansible

A practical Kubernetes platform built around the problems that come with running a cluster in a restricted environment.

![Kubernetes](https://img.shields.io/badge/Kubernetes-1.34.x-326CE5?logo=kubernetes&logoColor=white)
![Calico](https://img.shields.io/badge/CNI-Calico-0099CC)
![Ansible](https://img.shields.io/badge/Automation-Ansible-EE0000?logo=ansible&logoColor=white)
![Environment](https://img.shields.io/badge/Environment-Air--Gapped-informational)

</div>

---

## 🧭 What this project covers

This repository is the **central architecture repository** for a modular Kubernetes platform portfolio.

It documents the platform foundation and connects the specialized repositories that cover individual operational domains.

The platform includes:

- kubeadm-based cluster installation
- multiple control-plane nodes
- HAProxy + Keepalived for highly available Kubernetes API access
- Calico installed through the Tigera Operator
- NetworkPolicy and controlled workload traffic
- Longhorn persistent storage
- HAProxy Ingress Controller
- ClusterIP, NodePort and LoadBalancer services
- MetalLB for bare-metal load balancing where required
- ConfigMaps and Secrets for environment-specific configuration
- Metrics Server for Kubernetes resource metrics
- Lens for day-to-day cluster inspection
- Ansible for repeatable host and cluster operations
- offline images, packages and Helm artifacts
- controlled Kubernetes upgrades

The examples are sanitized. Organization names, internal addresses, domains, registry locations and credentials are never included.

---

## 🏛️ Platform portfolio architecture

This repository is intentionally **not a monolithic implementation repository**.

The platform is divided into focused repositories so that each major engineering concern can be developed, tested and demonstrated independently.

```text
                         Enterprise Kubernetes Platform
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
             Cluster Foundation   Networking         Security
                    │                 │                 │
                    └─────────────────┼─────────────────┘
                                      │
                    ┌─────────────────┼─────────────────┐
                    │                 │                 │
                    ▼                 ▼                 ▼
                 Storage          GitOps         Observability
                    │                 │                 │
                    └─────────────────┼─────────────────┘
                                      │
                                      ▼
                              Application Platform
                                      │
                                      ▼
                              Upgrade Automation
                                      │
                                      ▼
                               Air-Gapped Platform
```

Each specialized repository will contain the implementation for its own domain. This repository remains the **architectural entry point and system-level view**.

---

## 🧩 Project ecosystem

| Repository | Scope |
|---|---|
| `enterprise-kubernetes-platform` | Central architecture and platform overview |
| `kubernetes-production-cluster` | kubeadm, control plane, workers and cluster foundation |
| `kubernetes-networking` | Calico, NetworkPolicy, MetalLB, ingress and egress |
| `kubernetes-storage` | StorageClass, PV/PVC and stateful workloads |
| `kubernetes-security` | RBAC, Pod Security and workload isolation |
| `kubernetes-upgrade-automation` | Ansible-driven Kubernetes lifecycle and upgrades |
| `kubernetes-airgap` | Offline packages, images, registry and deployment |
| `kubernetes-gitops` | GitLab CI/CD, manifests and Argo CD |
| `kubernetes-observability` | Prometheus, Grafana, Filebeat, Logstash and Elasticsearch |
| `kubernetes-app-deployment` | Production-oriented application deployment patterns |

Repository links will be added as each specialized project is created and validated.

---

## 🏗️ Platform layout

```text
                         Client / kubectl / Lens
                                  │
                                  ▼
                         ┌─────────────────┐
                         │   HAProxy VIP   │
                         │   Keepalived    │
                         └────────┬────────┘
                                  │ :6443
                    ┌─────────────┼─────────────┐
                    ▼             ▼             ▼
                  CP-01         CP-02         CP-03
                    └─────────────┼─────────────┘
                                  │
                           ☸️ Kubernetes
                                  │
          ┌───────────────────────┼───────────────────────┐
          ▼                       ▼                       ▼
       🌐 Calico             🚪 HAProxy Ingress       💾 Longhorn
          │                       │                       │
   NetworkPolicy             Services                  PVC/PV
          │                       │                       │
          └───────────────────────┼───────────────────────┘
                                  ▼
                             Applications
```

The HAProxy/Keepalived pair protects access to the Kubernetes API. The Kubernetes control plane remains a separate HA layer.

---

## 🚀 Cluster bootstrap

The cluster is built with kubeadm rather than a pre-packaged Kubernetes distribution.

Typical flow:

```text
Prepare nodes
     ↓
Install required packages
     ↓
Initialize first control plane
     ↓
Join additional control planes
     ↓
Join workers
     ↓
Install CNI
     ↓
Validate cluster
```

The repository keeps cluster-specific values outside reusable manifests and automation where possible.

---

## 🌐 Calico networking

Calico is installed and managed through the **Tigera Operator**.

```text
Tigera Operator
      │
      ▼
Calico Installation
      │
 ┌────┴─────┐
 ▼          ▼
CNI     NetworkPolicy
```

The network layer is also where several operational considerations belong: version compatibility, policy behavior, workload connectivity and advanced networking capabilities.

Detailed networking implementation will be maintained in the dedicated `kubernetes-networking` repository.

---

## 🛡️ NetworkPolicy

The cluster uses restrictive workload communication rather than allowing every workload to communicate freely.

```text
Application A ───────► Application B
       │                    ▲
       │                    │
       └─── NetworkPolicy ──┘

Required traffic → ALLOW
Unrequired traffic → DENY
```

Policies should describe the traffic an application needs, not simply open the whole namespace.

---

## 🚦 Kubernetes services

The platform uses the normal Kubernetes service types according to the traffic requirement:

```text
ClusterIP
   │
   └── internal service communication

NodePort
   │
   └── explicit node-level exposure

LoadBalancer
   │
   └── external exposure where supported
```

For bare-metal environments, MetalLB can provide LoadBalancer behavior without depending on a cloud provider.

---

## 🚪 Ingress

External application traffic follows a separate path from Kubernetes API traffic.

```text
External traffic
      │
      ▼
Load Balancer / NodePort
      │
      ▼
HAProxy Ingress Controller
      │
      ▼
Kubernetes Service
      │
      ▼
Pod
```

This separation makes it easier to troubleshoot whether a problem is at the external load-balancing layer, ingress layer, service layer or application layer.

---

## 💾 Longhorn storage

Longhorn provides persistent volumes for workloads that need storage beyond the pod lifecycle.

```text
Application
     │
     ▼
    PVC
     │
     ▼
Longhorn Volume
     │
     ▼
 Persistent Data
```

Storage is treated as part of workload design. A PVC being `Pending`, a node becoming unavailable, or a scheduling constraint can all affect the application, so these layers need to be checked together.

Detailed storage implementation will be maintained in the dedicated `kubernetes-storage` repository.

---

## ⚙️ Configuration without rebuilding images

Application images should not contain environment-specific addresses.

Instead:

```text
              Generic Application Image
                         │
                         ▼
                    Application
                     ▲       ▲
                     │       │
                ConfigMap  Secret
                     │       │
              non-sensitive  sensitive
              configuration  values
```

This allows the same image to be deployed in different environments while changing configuration through Kubernetes resources.

Secrets are represented only with placeholders in this repository.

---

## 📊 Metrics and operations

Metrics Server provides Kubernetes resource metrics used for operational visibility and commands such as:

```bash
kubectl top nodes
kubectl top pods -A
```

Lens can be used as an additional operational UI for inspecting nodes, pods, deployments, services, events, resource usage and workload state.

No real kubeconfig, certificate, API endpoint or credentials are stored in the repository.

Detailed observability architecture will be maintained in the dedicated `kubernetes-observability` repository.

---

## 🧰 Ansible automation

Ansible is used where host preparation and cluster lifecycle operations need to be repeatable.

```text
Ansible
   │
   ├── inventory
   ├── group variables
   ├── roles
   └── playbooks
          │
          ▼
     Cluster nodes
```

The important part is repeatability: a node should not depend on a long list of undocumented manual changes before it can become part of the platform.

---

## 🔄 Upgrade approach

Kubernetes upgrades are handled as a controlled sequence rather than changing every node at once.

```text
Pre-check
   ↓
Prepare offline artifacts
   ↓
Control-plane upgrade
   ↓
Validate
   ↓
Worker upgrade
   ↓
Validate workloads
```

Version variables and upgrade steps are kept explicit so that the target version is easy to review before changing the cluster.

The complete lifecycle automation will be maintained in `kubernetes-upgrade-automation`.

---

## 📴 Offline operation

The platform was designed for environments where internet access cannot be assumed.

```text
Online preparation
      │
 ┌────┼──────────┐
 ▼    ▼          ▼
Images Packages Helm
 └────┼──────────┘
      ▼
Offline transfer
      ▼
Restricted environment
      ▼
Kubernetes
```

The repository contains only sanitized examples. Real registry addresses, package repositories and internal infrastructure details are excluded.

The complete offline deployment workflow will be maintained in `kubernetes-airgap`.

---

## 🧯 Troubleshooting approach

Most Kubernetes problems become easier when the traffic or resource path is followed from the outside inward.

### Pod is Pending

```text
Pod
 ↓
Scheduler events
 ↓
Node availability
 ↓
Taints / affinity / selectors
 ↓
PVC
 ↓
Storage
```

### Application is unreachable

```text
Client
 ↓
Load balancer
 ↓
Ingress
 ↓
Service
 ↓
Endpoint
 ↓
Pod
 ↓
NetworkPolicy
```

### Cluster upgrade issue

```text
Version
 ↓
Packages
 ↓
Images
 ↓
kubeadm
 ↓
Control plane
 ↓
Node state
 ↓
Workloads
```

The repository will record short problem/fix notes for issues that were actually encountered instead of turning every component into a long tutorial.

---

## 📁 Central repository structure

```text
enterprise-kubernetes-platform/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── networking.md
│   ├── security.md
│   ├── storage.md
│   ├── gitops.md
│   └── observability.md
├── diagrams/
├── projects/
└── .gitignore
```

Implementation repositories remain independent. This repository provides the system-level architecture, documentation and cross-project relationships.

---

## 🔐 Sanitization rule

Before anything is committed, check for:

```text
❌ organization names
❌ internal domains
❌ real IP addresses / CIDRs
❌ internal hostnames
❌ registry addresses
❌ application names
❌ usernames
❌ passwords / tokens
❌ private keys / certificates
❌ internal repository paths

✅ generic names
✅ placeholders
✅ fake example values
```

The public repository is intentionally separated from production infrastructure data.

---

## 🧭 Engineering principles

The platform is built around these principles:

1. **Separate concerns** — cluster foundation, networking, storage, security, GitOps and observability have independent ownership boundaries.
2. **Automate repeatable work** — Ansible and GitOps reduce undocumented manual operations.
3. **Design for restricted environments** — internet access is never treated as a prerequisite for runtime operations.
4. **Validate every layer** — node, runtime, Kubernetes, networking, storage and workload health are checked independently.
5. **Prefer explicit configuration** — versions, endpoints and operational assumptions should be visible and reviewable.
6. **Keep production data out of Git** — credentials and infrastructure-specific identifiers are never committed.

---

<div align="center">

### ☸️ Build → Secure → Automate → Upgrade → Operate

**A modular Kubernetes platform portfolio built from real operational patterns.**

</div>
