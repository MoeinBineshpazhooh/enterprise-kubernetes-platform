# Calico networking

Calico is the cluster CNI and is managed through the Tigera Operator.

```text
Tigera Operator
      │
      ▼
Calico components
      │
 ┌────┴────┐
 ▼         ▼
CNI    NetworkPolicy
```

The networking layer is responsible for pod connectivity and policy enforcement.

## Operational notes

Version compatibility matters when upgrading Kubernetes or Calico. Changes to the CNI should be validated before moving on to workload networking.

Advanced Calico capabilities were evaluated separately from the basic CNI and policy configuration.

No production cluster addresses or organization-specific values are stored here.
