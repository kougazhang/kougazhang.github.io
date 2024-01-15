---
script: true
---
# k8s concepts

## Objects in Kubernetes

### Namespaces

In Kubenetes, *namespaces* provides a mechanism for isolating group of resources within a single cluster.

List the current namespaces in a cluster using:

```
kubectl get namespaces
```

When you create a Service, it creates a corresponding DNS entry.
This entry is of the form
`<service-name>.<namespace-name>.svc.cluster.local`.
