<div align="center">
  <img src="https://github.com/MoeinBineshpazhooh.png?size=160" width="120" alt="Moein Bineshpazhooh" />
  <h1>☸️ Enterprise Kubernetes & DevOps Platform</h1>
  <p><strong>On-Prem Kubernetes • Networking • Storage • Security • GitOps • Observability • Kafka</strong></p>
</div>

<div align="center">

![Kubernetes](https://img.shields.io/badge/Kubernetes-Production-326CE5?logo=kubernetes&logoColor=white)
![Ansible](https://img.shields.io/badge/Automation-Ansible-EE0000?logo=ansible&logoColor=white)
![GitLab](https://img.shields.io/badge/CI%2FCD-GitLab-FC6D26?logo=gitlab&logoColor=white)
![Argo CD](https://img.shields.io/badge/GitOps-Argo%20CD-EF7B4D?logo=argo&logoColor=white)
![Calico](https://img.shields.io/badge/Networking-Calico-0099CC)
![Kafka](https://img.shields.io/badge/Streaming-Kafka-231F20?logo=apachekafka&logoColor=white)
![Air-Gapped](https://img.shields.io/badge/Environment-Air--Gapped-111827)

</div>

---

## 🎯 What this portfolio demonstrates

This repository is the **central architecture and portfolio hub** for a practical DevOps platform built around real-world on-premises and restricted-environment operations.

The objective is not to present isolated technology tutorials. It is to demonstrate the complete engineering lifecycle:

```text
Infrastructure
      ↓
Kubernetes Cluster
      ↓
Networking / Storage / Security
      ↓
Automation / GitOps
      ↓
Observability / Logging
      ↓
Kafka / Platform Services
      ↓
Application Delivery
      ↓
Operations / Troubleshooting / Recovery
```

The implementation repositories are intentionally separated by operational domain while this repository keeps the **system-level architecture and relationships** visible.

---

## 🧑‍💻 On-Premises Kubernetes experience

The Kubernetes foundation is designed around the realities of operating clusters outside managed cloud services:

- kubeadm-based cluster bootstrap
- multi-control-plane high availability
- HAProxy Kubernetes API load balancing
- Keepalived virtual IP failover
- Ubuntu node preparation
- containerd runtime
- control-plane and worker lifecycle
- CNI networking
- NetworkPolicy enforcement
- MetalLB / bare-metal LoadBalancer capability
- ingress traffic management
- persistent storage
- Ansible-based repeatable operations
- controlled Kubernetes upgrades
- restricted and air-gapped environments
- failure testing, troubleshooting and recovery

The dedicated cluster repository presents this as an **On-Premises Kubernetes Deployment & Operations case study**, rather than simply a Kubernetes configuration collection.

---

## 🏛️ Platform architecture

```text
                         Enterprise Platform
                                  │
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
   Infrastructure             Platform                Operations
          │                       │                       │
   ┌──────┼──────┐          ┌─────┼─────┐          ┌─────┼─────┐
   ▼      ▼      ▼          ▼     ▼     ▼          ▼     ▼     ▼
 K8s   Network Storage    GitOps Security Apps   Kafka Observability
   │      │      │          │     │     │          │       │
   └──────┴──────┴──────────┴─────┴─────┴──────────┴───────┘
                                  │
                                  ▼
                       Production Operations
```

---

## 🧩 Project ecosystem — 10 focused repositories

| # | Repository | Primary capability |
|---:|---|---|
| 1 | `kubernetes-production-cluster` | **On-prem Kubernetes deployment, kubeadm and HA control plane** |
| 2 | `kubernetes-networking` | **Calico, NetworkPolicy, MetalLB, ingress and egress** |
| 3 | `kubernetes-storage` | **Longhorn, PV/PVC and stateful workload storage** |
| 4 | `kubernetes-security` | **RBAC, Pod Security, isolation and workload boundaries** |
| 5 | `kubernetes-upgrade-automation` | **Ansible + kubeadm lifecycle and upgrades** |
| 6 | `kubernetes-airgap` | **Offline packages, images, registry and restricted deployment** |
| 7 | `kubernetes-gitops` | **GitLab CI/CD + Argo CD + deployment automation** |
| 8 | `kubernetes-observability` | **Prometheus, Grafana, Filebeat, Logstash and Elasticsearch** |
| 9 | `kafka-production-platform` | **Kafka KRaft, SASL, ACLs and operational troubleshooting** |
| 10 | `enterprise-devops-platform` | **Application delivery from source code to production** |

This central repository remains the **architecture layer**, while each specialized repository provides deeper implementation evidence.

---

## 🔗 Portfolio relationship

```text
                         ┌──────────────────────────────┐
                         │ Enterprise Kubernetes       │
                         │ & DevOps Platform            │
                         │ CENTRAL ARCHITECTURE         │
                         └──────────────┬───────────────┘
                                        │
              ┌─────────────────────────┼─────────────────────────┐
              │                         │                         │
              ▼                         ▼                         ▼
      ON-PREM INFRASTRUCTURE       PLATFORM SERVICES       DELIVERY & OPS
              │                         │                         │
        K8s / Networking          Storage / Security       GitOps / CI-CD
        HA / Runtime              Kafka / Observability   Applications
        Air-Gap / Upgrades
```

---

## 🚀 Deployment lifecycle

```text
Physical / VM infrastructure
          ↓
Ubuntu preparation
          ↓
Containerd
          ↓
HAProxy + Keepalived
          ↓
kubeadm bootstrap
          ↓
Control-plane expansion
          ↓
Worker expansion
          ↓
Calico networking
          ↓
Storage / ingress / services
          ↓
Security policies
          ↓
Observability
          ↓
GitOps / application delivery
          ↓
Validation / failure testing
          ↓
Production operations
```

This separation is important: a production platform is not just a running Kubernetes API. It is the complete operational path around the cluster.

---

## 🌐 Networking layer

The networking domain covers:

```text
Calico
  ├── CNI
  ├── NetworkPolicy
  └── advanced networking

MetalLB
  └── bare-metal LoadBalancer

HAProxy Ingress
  └── application traffic

Kubernetes Services
  ├── ClusterIP
  ├── NodePort
  └── LoadBalancer
```

The dedicated networking repository will contain the implementation details and troubleshooting patterns.

---

## 💾 Storage layer

Persistent workloads are treated as an operational concern rather than an afterthought.

```text
Application
     ↓
PVC
     ↓
StorageClass
     ↓
Longhorn
     ↓
Persistent Data
```

The storage project will demonstrate provisioning, scheduling interactions, failure behavior and recovery considerations.

---

## 🛡️ Security layer

Security is built around explicit boundaries:

- RBAC
- least-privilege access
- namespace and workload isolation
- Pod Security controls
- NetworkPolicy
- Secret handling
- separation of production values from public source code

No production credentials, private keys, real internal addresses or sensitive infrastructure identifiers belong in these repositories.

---

## ⚙️ Automation & lifecycle

Ansible is used for repeatable infrastructure and cluster operations, while kubeadm provides explicit Kubernetes lifecycle control.

```text
Inventory
   ↓
Variables
   ↓
Roles
   ↓
Playbooks
   ↓
Node preparation / lifecycle
   ↓
Validation
```

The upgrade project will focus on controlled version changes, pre-checks, offline artifacts, sequencing and post-upgrade validation.

---

## 🔄 GitOps & application delivery

The delivery layer connects source control with Kubernetes operations:

```text
Developer
   ↓
GitLab
   ↓
CI/CD
   ↓
Container Image
   ↓
Private Registry
   ↓
GitOps Repository
   ↓
Argo CD
   ↓
Kubernetes
   ↓
Application
```

The goal is a traceable deployment path rather than a manually operated production cluster.

---

## 📊 Observability & logging

The observability domain combines metrics, dashboards and centralized logging:

```text
Applications / Nodes
        │
        ├──────────────► Prometheus ──► Grafana
        │
        └──────────────► Filebeat
                              ↓
                           Kafka
                              ↓
                          Logstash
                              ↓
                        Elasticsearch
                              ↓
                           Kibana
```

Operational troubleshooting is treated as part of the platform, including file rotation, registry state, permissions, ingestion paths and downstream indexing.

---

## 📨 Kafka platform

Kafka is a separate platform capability covering:

- KRaft architecture
- broker/controller separation
- SASL authentication
- ACL-based authorization
- service-specific identities
- client connectivity
- AKHQ administration
- replication and metadata behavior
- operational troubleshooting
- restricted-environment deployment

The objective is to demonstrate **operating Kafka as infrastructure**, not merely producing and consuming messages.

---

## 📴 Air-gapped engineering

Restricted environments are treated as a first-class design constraint.

```text
Internet-connected preparation
          ↓
Packages / images / Helm / artifacts
          ↓
Offline transfer
          ↓
Private registry / local repository
          ↓
Air-gapped cluster
          ↓
Installation / upgrade / operations
```

This approach applies across Kubernetes, container runtime, observability and Kafka components.

---

## 🧯 Operational problem-solving

The portfolio will emphasize actual engineering behavior:

```text
Symptom
  ↓
Observe
  ↓
Isolate the layer
  ↓
Validate assumptions
  ↓
Change one boundary
  ↓
Re-test
  ↓
Document root cause
  ↓
Automate prevention
```

This is intentionally different from presenting only successful deployment commands.

---

## 🔐 Public repository hygiene

All public examples follow a strict sanitization model:

```text
❌ Real credentials
❌ Tokens / private keys
❌ Internal domains
❌ Real production IPs
❌ Internal registry addresses
❌ Organization-specific identifiers
❌ Private application names

✅ Generic hostnames
✅ Placeholders
✅ Fake example values
✅ Reusable architecture
✅ Sanitized operational lessons
```

---

## 🧭 Engineering principles

1. **Infrastructure first** — establish a reliable foundation before layering platform services.
2. **High availability by design** — remove avoidable single points of failure.
3. **Automation over repetition** — encode repeatable operations in Ansible and GitOps.
4. **Explicit lifecycle management** — make versions, dependencies and upgrade sequences reviewable.
5. **Layered troubleshooting** — isolate failures before changing multiple components.
6. **Offline awareness** — do not assume unrestricted internet connectivity.
7. **Operational evidence** — document validation, failure scenarios and recovery paths.
8. **Security by boundary** — keep access, traffic and configuration explicitly controlled.
9. **Portfolio modularity** — each repository demonstrates one meaningful engineering capability.

---

<div align="center">

### ☸️ Build → Secure → Automate → Observe → Deliver → Operate

**A modular DevOps portfolio built around real on-premises infrastructure experience.**

</div>
